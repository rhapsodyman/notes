
### Common commands
```sh
$ hostname                   # show or set system's hostname
$  history | grep netstat    # search the command  history
$ cd -                       # navigate to previous directory
$ Ctrl + A                   # navigate to start of command
$ Ctrl + E                   # navigate to the end of command
$_ shell variable            # in bash, is the last argument given to the previous command
!$ shell variable            # in bash, gives the last argument of previous command in the shell history
mkdir -p a/b/c/d/e && cd $_  # create folder and switch to it
$! shell variable            # the PID of the last executed command
du -ch                       # estimate file space usage
su -                         # switch to root user
```


### Network commands
```sh
$ ifconfig                   # configure/view network interfaces
$ ping -c 5 www.google.com   # ping 5 times
$ arp                        # manipulate the system ARP cache
```


### DNS configuration issues

**ping: www.google.com: Temporary failure in name resolution**

* https://stackoverflow.com/questions/53687051/ping-google-com-temporary-failure-in-name-resolution
* https://www.tecmint.com/set-permanent-dns-nameservers-in-ubuntu-debian/
 
    On modern Linux systems that use systemd (system and service manager), the DNS or name resolution services are provided to local applications via the systemd-resolved service. 
    It uses the systemd DNS stub file (**/run/systemd/resolve/stub-resolv.conf**) in the default mode of operation.
    The DNS stub file contains the local stub **127.0.0.53** as the only DNS server.
    
    ```ls -l /etc/resolv.conf``` - you will see that this file is a symlink to the /run/systemd/resolve/stub-resolv.conf file.
    Unfortunately, because the /etc/resolv.conf is indirectly managed by the systemd-resolved service, any changes made manually by a user can not be saved permanently or only last for a while.
    
    Quick fix - just to modify the **/etc/resolv.conf** to 8.8.8.8



## Open VPN server
Install and configure 
* https://interface31.ru/tech_it/2019/10/nastroyka-openvpn-servera-dlya-dostupa-v-internet.html  (russian)

**restart**
```
sudo systemctl stop openvpn@server
sudo systemctl enable openvpn@server.service
sudo systemctl start openvpn@server
```


## Start a python2 web server

 ``` python -m SimpleHTTPServer 80```
 
 
 ## Pen Testing
 * **Target validation** - WHOIS, nslookup, dnsrecon
 * **Finding subdomains** - Google Fu, dig, Nmap, Sublist3r, Bluto, crt/sh, etc
 * **Fingerprinting** - nmap, Wappalyzer, BuiltWith, Netcat
 * **Data Breaches** - HaveIBeenPwned and similar

**TCP**  - SYN - SYN ACK - ACK 
**nmap** -  SYN - SYN ACK - RST
**nmap "ping scan"** - ``` nmap -sn 10.0.2.0/24```  show all the nodes in a network
```nmap -T4 10.0.2.2``` - top 1000 widely used ports scanned 
```nmap -T4 -p-``` - all ports scanned 
```nmap -T4 -p22,80,443``` - selective ports scanned (can add here ```-A``` for more info)
```/usr/share/nmap/scripts``` - namp usefull scripts


