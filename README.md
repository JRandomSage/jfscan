![logo](screenshots/jfscan_logo.png)
# Description
The JFScan (Just Fu*king Scan) is a Masscan wrapper designed to simplify work when scanning for open ports on mixed targets, inluding domain names. Some useful modules are included, such as modules for subdomain enumeration using Amass and crt.sh. The JFScan accepts a target in the following forms: URL, domain or IP (including CIDR). You can specify a file with targets using argument or just use stdin. The JFScan also allows you to output only the results and chain it with other tools, for example Nuclei.

# Installation
A python3 is required.

```
$ git clone https://github.com/nullt3r/jfscan.git
$ cd jfscan
$ python3 setup.py install
```

# Usage

```
usage: __main__.py [-h] -t TARGETS [-m MODULES] (-p PORTS | -tp TOP_PORTS) [-r MAX_RATE] [-oi] [-od] [-q]

JFScan - Just Fu*king Scan

optional arguments:
  -h, --help            show this help message and exit
  -t TARGETS, --targets TARGETS
                        list of targets, accepted form is: domain name, IPv4, IPv6, URL
  -m MODULES, --modules MODULES
                        modules separated by a comma, available modules: enum_amass, enum_crtsh
  -p PORTS, --ports PORTS
                        ports, can be a range or port list: 0-65535 or 22,80,100-500,...
  -tp TOP_PORTS, --top-ports TOP_PORTS
                        scan only N of the top ports, e. g., --top-ports 1000
  -r MAX_RATE, --max-rate MAX_RATE
                        max kpps rate
  -oi, --only-ips       output only IP adresses, default: all resources
  -od, --only-domains   output only domains, default: all resources
  -q, --quite           output only results
```

## Example
![example](screenshots/sample_scan.png)


Scan targets for only for ports 80 and 443 with rate of 10 kpps:

`$ jfscan -p 80,443 -t targets.txt -r 10000`

Scan targets for only for ports 80 and 443 and utilize a crt.sh subdomain enumeration modules:

`$ jfscan -p 80,443 -t targets.txt -m enum_crtsh`

Scan targets for top 1000 ports and utilize crt.sh module:

`$ jfscan --top-ports 1000 -t targets.txt -m enum_crtsh`

You can also specify targets on stdin and pipe it to nuclei:

`$ cat targets.txt | jfscan --top-ports 1000 -m enum_crtsh | nuclei`

The targets.txt can contain targets in the following forms:
```
http://domain.com/
domain.com
1.2.3.4
1.2.3.0/24
```

The CIDR format is not supported, but will be implemented in the next release.

# License
Read file LICENSE.


# Disclaimer
I am not responsible for any damages. You are responsible for your own
actions. Attacking targets without prior mutual consent is illegal.