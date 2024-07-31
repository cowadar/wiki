# Find commando

## Lijst generen van alle folders in een bepaalde directory

```bash
find . -maxdepth 1 -type d -printf '%f\n'
```

## Zoek alle files (-type f) in map youtube met in de naam trailer

```sh
find ./youtube -type f -name '*trailer*'
```

### Delete al deze files

```sh
find ./youtube -type f -name '*trailer*' -delete
```
