---
title: Synology
---

## Install Nano on Synology
This post will show you how to install [Nano](https://en.wikipedia.org/wiki/GNU_nano) on your [Synology](https://www.synology.com/) [NAS](https://anto.online/vocabulary/nas/). Many people prefer Nano since Nano is easier to use than [VIM](https://anto.online/vocabulary/vim/). Luckily the [Synology](https://anto.online/vocabulary/synology/) NAS allows you to add additional software repositories and install Nano easily.

#### Enable SSH Access

You must enable [SSH](https://anto.online/vocabulary/ssh/) access to use Nano on your Synology NAS.

Click on the ‘Control Panel’ icon and then click on ‘[Terminal](https://anto.online/vocabulary/terminal/) & [SNMP](https://anto.online/vocabulary/snmp/)’. Finally, tick the option called ‘Enable SSH Service’ and click ‘apply’.

![Synology NAS, Terminal & SNMP, Enable SSH Service](https://anto.online/wp-content/uploads/2020/01/Synology-NAS-Terminal-SNMP-Enable-SSH-Service-559x300.png)

Synology NAS, Terminal & SNMP, Enable SSH Service

#### Add a repository

Nano is not provided or maintained by Synology, but you can get it from the SynoCommunity [repository](https://anto.online/vocabulary/repository/). In addition, SynoCommunity provides free packages for Synology NAS devices. Visit them at [https://synocommunity.com/](https://synocommunity.com/) and feel free to explore some of the other packages they have to offer, like [Git](https://anto.online/vocabulary/git/), Home Assistant, and of course, Nano.

To add the SynoCommunity repository:

Click on the ‘Package Center’ icon and then click on the ‘settings’ button.

In the ‘settings’ window ‘general’ tab, click on the ‘Synology Inc and trusted publishers’ radio button in the’ trust level’ section.

![Synology NAS, Package Center, Settings, Trust Level](https://anto.online/wp-content/uploads/2020/01/Synology-NAS-Package-Center-Settings-Trust-Level-535x300.png)

Synology NAS, Package Center, Settings, Trust Level

On the ‘settings’ window ‘package sources’ tab, click on the ‘Add’ button and add the repository:
Name: SynoCommunity
Location: [http://packages.synocommunity.com/](http://packages.synocommunity.com/)

Click on the ‘Ok’ button once you are done.

![Synology NAS, Package Center, Settings, Add Repository](https://anto.online/wp-content/uploads/2020/01/Synology-NAS-Package-Center-Settings-Add-Repositry-534x300.png)

Synology NAS, Package Center, Settings, Add Repository

#### Install Nano from the Repository

The final step is to install Nano. To do this, make sure you are still in the ‘package centre’. Then, click on the ‘community’ section, find ‘SynoCli File Tools’ and click on the ‘install’ button. The SynoCli File Tools package provides the following command-line utilities: [less](https://www.man7.org/linux/man-pages/man1/less.1.html), [tree](https://man7.org/linux/man-pages/man1/git-read-tree.1.html), [ncdu](https://linux.die.net/man/1/ncdu), [jdupes](https://manpages.debian.org/testing/jdupes/jdupes.1.en.html), [rhash](https://manpages.debian.org/jessie/rhash/rhash.1.en.html), [mc](https://linux.die.net/man/1/mc), [nano](https://linux.die.net/man/1/nano), [file](https://man7.org/linux/man-pages/man1/file.1.html), [detox](https://linux.die.net/man/1/detox), [rmlint](https://manpages.debian.org/testing/rmlint/rmlint.1.en.html), and [rnm](https://man7.org/linux/man-pages/man1/rm.1.html) v1.1-3. Nano was previously a separate package but is now only available via the SynoCli File Tools package.

#### Wrapping up

You can now run Nano using SSH in the usual way! Type ‘nano’ and the file name you wish to open. Remember that you will need ‘sudo’ in some circumstances, depending on what you need to do.
