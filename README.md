# SSL Checker
#### Simple Python script that collects SSL information from hosts

## About

It's a simple script running in python that collects SSL information then it returns the group of information in JSON. It can also connects trough your specified SOCKS server.

## Requirements

You only need to installl pyOpenSSL:

`pip install pyopenssl`

## Usage

```
./ssl_checker.py -h
usage: ssl_checker.py -H [HOSTS [HOSTS ...]] [-s HOST:PORT] [-j] [-p] [-h]

optional arguments:
  -H [HOSTS [HOSTS ...]], --host [HOSTS [HOSTS ...]]
                        Hosts as input separated by space
  -s HOST:PORT, --socks HOST:PORT
                        Enable SOCKS proxy for connection
  -j, --json            Enable JSON in the output
  -p, --pretty          Print pretty and more human readable Json
  -h, --help            Show this help message and exit
```



Port is optional here. The script will use 443 if not specified.

`-j, --json`	Use this if you want to only have the result in JSON

`-p, --pretty` Use this with `-j` to print indented and human readable json

`-s, --socks` Enable connection through SOCKS server

`-H, --host`	Enter the hosts separated by space

`-h, --help` Shows the help and exit

## Censored?

No problem. Pass `-s/--socks` argument to the script as `HOST:PORT` to connect through SOCKS proxy.

```
narbeh@narbeh-xps:~/ssl-checker$ ./ssl_checker.py -H facebook.com
Analyzing 1 host(s):
--------------------

	[-] facebook.com         Failed: [Errno 111] Connection refused
	----

0 successful and 1 failed

narbeh@narbeh-xps:~/ssl-checker$ ./ssl_checker.py -H facebook.com -s localhost:9050
Analyzing 1 host(s):
--------------------

	[+] facebook.com

		Issued domain: *.facebook.com
		Issued by: DigiCert Inc
		Valid from: 2017-12-15
		Valid to: 2019-03-22 (334 days left)
		Validity days: 462
		Certificate S/N: 14934250041293165463321169237204988608
		Certificate version: 2
		Certificate algorithm: sha256WithRSAEncryption
		Expired: False
	----

1 successful and 0 failed

```




## Example

```
narbeh@narbeh-xps:~/ssl-checker$ ./ssl_checker.py -H narbeh.org google.com:443 facebook.com
Analyzing 3 host(s):
-------------------

	[+] narbeh.org

		Issued domain: narbeh.org
		Issued by: Let's Encrypt
		Valid from: 2018-04-21
		Valid to: 2018-07-20 (89 days left)
		Validity days: 90
		Certificate S/N: 338163108483756707389368573553026254634358
		Certificate version: 2
		Certificate algorithm: sha256WithRSAEncryption
		Expired: False
	----
	[+] google.com

		Issued domain: *.google.com
		Issued by: Google Inc
		Valid from: 2018-03-28
		Valid to: 2018-06-20 (59 days left)
		Validity days: 83
		Certificate S/N: 2989116342670522968
		Certificate version: 2
		Certificate algorithm: sha256WithRSAEncryption
		Expired: False
	----
	[-] facebook.com         Failed: [Errno 111] Connection refused
	----

2 successful and 1 failed
```


Example only with the `-j/--json` and `-p/--pretty` arguments which shows the JSON only. Perfect for piping to another tool.

```
narbeh@narbeh-xps:~/ssl-checker$ ./ssl_checker.py -j -p -H  narbeh.org:443 test.com
{'narbeh.org': {'cert_alg': u'sha256WithRSAEncryption',
                'cert_exp': False,
                'cert_sn': 338163108483756707389368573553026254634358L,
                'cert_ver': 2,
                'issued_to': u'narbeh.org',
                'issuer_c': u'US',
                'issuer_cn': u"Let's Encrypt Authority X3",
                'issuer_o': u"Let's Encrypt",
                'issuer_ou': None,
                'valid_from': '2018-04-21',
                'valid_till': '2018-07-20',
                'validity_days': 90},
 'test.com': {'cert_alg': u'sha256WithRSAEncryption',
              'cert_exp': False,
              'cert_sn': 73932709062103623902948514363737041075L,
              'cert_ver': 2,
              'issued_to': u'www.test.com',
              'issuer_c': u'US',
              'issuer_cn': u'Network Solutions DV Server CA 2',
              'issuer_o': u'Network Solutions L.L.C.',
              'issuer_ou': None,
              'valid_from': '2017-01-15',
              'valid_till': '2020-01-24',
              'validity_days': 1104}}
```
