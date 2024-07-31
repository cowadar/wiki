# Pihole

## Setting up Pi-hole as a recursive DNS server solution

We will use [`unbound`](https://github.com/NLnetLabs/unbound), a secure open-source recursive DNS server primarily developed by NLnet Labs, VeriSign Inc., Nominet, and Kirei. The first thing you need to do is to install the recursive DNS resolver:

```bash
sudo apt install unbound
```

If you are installing unbound from a package manager, it should install the `root.hints` file automatically with the dependency `dns-root-data`. The root hints will then be automatically updated by your package manager.

**Optional**: Download the current root hints file (the list of primary root servers which are serving the domain "." - the root domain). Update it roughly every six months. Note that this file changes infrequently. This is only necessary if you are not installing unbound from a package manager. If you do this optional step, you will need to uncomment the `root-hints:` configuration line in the suggested config file.

```bash
wget https://www.internic.net/domain/named.root -qO- | sudo tee /var/lib/unbound/root.hints
```

### Configure `unbound`

Highlights:

- Listen only for queries from the local Pi-hole installation (on port 5335)
- Listen for both UDP and TCP requests
- Verify DNSSEC signatures, discarding BOGUS domains
- Apply a few security and privacy tricks

`/etc/unbound/unbound.conf.d/pi-hole.conf`:

```sh
server:
    # If no logfile is specified, syslog is used
    # logfile: "/var/log/unbound/unbound.log"
    verbosity: 0

    interface: 127.0.0.1
    port: 5335
    do-ip4: yes
    do-udp: yes
    do-tcp: yes

    # May be set to yes if you have IPv6 connectivity
    do-ip6: no

    # You want to leave this to no unless you have *native* IPv6. With 6to4 and
    # Terredo tunnels your web browser should favor IPv4 for the same reasons
    prefer-ip6: no

    # Use this only when you downloaded the list of primary root servers!
    # If you use the default dns-root-data package, unbound will find it automatically
    #root-hints: "/var/lib/unbound/root.hints"

    # Trust glue only if it is within the server's authority
    harden-glue: yes

    # Require DNSSEC data for trust-anchored zones, if such data is absent, the zone becomes BOGUS
    harden-dnssec-stripped: yes

    # Don't use Capitalization randomization as it known to cause DNSSEC issues sometimes
    # see https://discourse.pi-hole.net/t/unbound-stubby-or-dnscrypt-proxy/9378 for further details
    use-caps-for-id: no

    # Reduce EDNS reassembly buffer size.
    # IP fragmentation is unreliable on the Internet today, and can cause
    # transmission failures when large DNS messages are sent via UDP. Even
    # when fragmentation does work, it may not be secure; it is theoretically
    # possible to spoof parts of a fragmented DNS message, without easy
    # detection at the receiving end. Recently, there was an excellent study
    # >>> Defragmenting DNS - Determining the optimal maximum UDP response size for DNS <<<
    # by Axel Koolhaas, and Tjeerd Slokker (https://indico.dns-oarc.net/event/36/contributions/776/)
    # in collaboration with NLnet Labs explored DNS using real world data from the
    # the RIPE Atlas probes and the researchers suggested different values for
    # IPv4 and IPv6 and in different scenarios. They advise that servers should
    # be configured to limit DNS messages sent over UDP to a size that will not
    # trigger fragmentation on typical network links. DNS servers can switch
    # from UDP to TCP when a DNS response is too big to fit in this limited
    # buffer size. This value has also been suggested in DNS Flag Day 2020.
    edns-buffer-size: 1232

    # Perform prefetching of close to expired message cache entries
    # This only applies to domains that have been frequently queried
    prefetch: yes

    # One thread should be sufficient, can be increased on beefy machines. In reality for most users running on small networks or on a single machine, it should be unnecessary to seek performance enhancement by increasing num-threads above 1.
    num-threads: 1

    # Ensure kernel buffer is large enough to not lose messages in traffic spikes
    so-rcvbuf: 1m

    # Ensure privacy of local IP ranges
    private-address: 192.168.0.0/16
    private-address: 169.254.0.0/16
    private-address: 172.16.0.0/12
    private-address: 10.0.0.0/8
    private-address: fd00::/8
    private-address: fe80::/10
```

Start your local recursive server and test that it's operational:

```sh
sudo service unbound restart
dig pi-hole.net @127.0.0.1 -p 5335
```

!!! warning
    Note that you first have to install dnsutils (sudo apt install dnsutils) if dig command is not found

The first query may be quite slow, but subsequent queries, also to other domains under the same TLD, should be fairly quick.

You should also consider adding

```bash
edns-packet-max=1232
```

to a config file like `/etc/dnsmasq.d/99-edns.conf` to signal FTL to adhere to this limit.

### Test validation[¶](https://docs.pi-hole.net/guides/dns/unbound//#test-validation "Permanent link")

You can test DNSSEC validation using

```
dig fail01.dnssec.works @127.0.0.1 -p 5335
dig dnssec.works @127.0.0.1 -p 5335
```

The first command should give a status report of `SERVFAIL` and no IP address. The second should give `NOERROR` plus an IP address.

### Configure Pi-hole

Finally, configure Pi-hole to use your recursive DNS server by specifying `127.0.0.1#5335` as the Custom DNS (IPv4):

![Upstream DNS Servers Configuration](https://docs.pi-hole.net/images/RecursiveResolver.png)

(don't forget to hit Return or click on `Save`)

## Web password terugvinden

```bash
cat /etc/pihole/setupVars.conf |grep WEBPASSWORD
```
