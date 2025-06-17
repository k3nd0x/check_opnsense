# check_opnsense
Icinga check command for OPNsense firewall monitoring

## Requirements

This check command depends on the following python modules:
 * enum
 * requests
 * argparse

**Installation on Debian / Ubuntu**
```
apt install python3 python3-requests
```

**Installation on Rocky / Alma Linux 9**
```
yum install python3 python3-requests
```

**Installation on FreeBSD**
```
pkg install python3 py39-requests
```

## Usage

The ``icinga2`` folder contains the command defintion and service examples for use with Icinga2.

```shell
usage: check_opnsense.py [-h] -H HOSTNAME [-p PORT] --api-key API_KEY --api-secret API_SECRET [-k] -m {updates,ipsec,hastatus,interfaces,services,wireguard} [-w TRESHOLD_WARNING] [-c TRESHOLD_CRITICAL]

Check command OPNsense firewall monitoring

options:
  -h, --help            show this help message and exit

API Options:
  -H HOSTNAME, --hostname HOSTNAME
                        OPNsense hostname or ip address
  -p PORT, --port PORT  OPNsense https-api port
  --api-key API_KEY     API key (See OPNsense user manager)
  --api-secret API_SECRET
                        API key (See OPNsense user manager)
  -k, --insecure        Don't verify HTTPS certificate
  -v, --verbose         Print verbose output (helpful at filtering)
  -f, --filter          String Option to filter some objects (Example --filter 'wg0,wg1')

Check Options:
  -m {updates,ipsec}, --mode {updates,ipsec}
                        Mode to use.
  -w TRESHOLD_WARNING, --warning TRESHOLD_WARNING
                        Warning treshold for check value
  -c TRESHOLD_CRITICAL, --critical TRESHOLD_CRITICAL
                        Critical treshold for check value

```

## Create API credentials

Go to the user manager and select the user you want to use for API access. Click the ``+`` icon in the ``API keys`` section to add a new API key, which triggers a download of a tex file containing the key and secret.

This file should look similar to this one:

```
key=w86XNZob/8Oq8aC5r0kbNarNtdpoQU781fyoeaOBQsBwkXUt
secret=XeD26XVrJ5ilAc/EmglCRC+0j2e57tRsjHwFepOseySWLM53pJASeTA3
```

For further information have a look at the [opnsense documentation](https://docs.opnsense.org/development/how-tos/api.html).

## Examples

**Check for updates**
```shell
./check_opnsense.py -H <OPNSENSE_HOSTNAME> --api-key <API_KEY> --api-secret <API_SECRET>  -m updates
CRITICAL - There are 43 updates available, total download size is 199.1MiB. This update requires a reboot.|upgrade_packages=42 reinstall_packages=1 remove_packages=0 available_updates=43

./check_opnsense.py -H <OPNSENSE_HOSTNAME> --api-key <API_KEY> --api-secret <API_SECRET>  -m updates
WARNING - There are 14 updates available, total download size is 64.8MiB.|upgrade_packages=14 reinstall_packages=0 remove_packages=0 available_updates=14

./check_opnsense.py -H <OPNSENSE_HOSTNAME> --api-key <API_KEY> --api-secret <API_SECRET>  -m updates
OK - System up to date|upgrade_packages=0 reinstall_packages=0 remove_packages=0 available_updates=0
```

***Check ipsec tunnel status***
```shell
./check_opnsense.py -H <OPNSENSE_HOSTNAME> --api-key <API_KEY> --api-secret <API_SECRET>  -m updates
[WARNING] IPsec tunnels not connected: headquarter

./check_opnsense.py -H <OPNSENSE_HOSTNAME> --api-key <API_KEY> --api-secret <API_SECRET>  -m updates
[OK] IPsec tunnels connected: remote-office, headquarter
```

***Check wireguard tunnel status***
```shell
./check_opnsense.py -H <OPNSENSE_HOSTNAME> --api-key <API_KEY> --api-secret <API_SECRET>  -m wireguard
[OK] 2/2 Wireguard peers are online
[OK] Peer host1 is online (8.8.8.4:35376)
[OK] Peer host2 is online (8.8.8.5:34376)
```