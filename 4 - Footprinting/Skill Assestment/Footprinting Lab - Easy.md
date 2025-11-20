Server is an Internal DNS Server.
Credentials found: ceil / qwer1234
Comapny's employees talking about SSH Keys.
flag.txt file on the server.
IP 10.129.121.114

First step is to do a Nmap scan  for open TCP Ports, in this case we can see if we have an SSH host open too.
┌─[eu-academy-5]─[10.10.15.130]─[htb-ac-450682@htb-9xw13f5qt5]─[~]
└──╼ [★]$ sudo nmap 10.129.121.114 --top-ports=10 
![Pasted image 20251120173639](../../IMAGES/Pasted%20image%2020251120173639.png)

We have open FTP (21) and SSH (22) Services. 
![Pasted image 20251120173748](../../IMAGES/Pasted%20image%2020251120173748.png)
## FTP Enumeration
We will start to enumerate FTP since we have a guide for it.
Next we will run a nmap script scan. -sV for a version scan, -A for aggresive scan and -sC for default script scan.
```
sudo nmap -sV -p21 -sC -A 10.129.121.114
```

No interesting finds from the scann
![Pasted image 20251120174345](../../IMAGES/Pasted%20image%2020251120174345.png)
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-11-20 10:41 CST
Nmap scan report for 10.129.121.114
Host is up (0.048s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     ProFTPD
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.8 (95%), Linux 5.0 (95%), Linux 5.0 - 5.4 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.5 (95%), Linux 3.1 (94%), Linux 3.2 (94%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), HP P2000 G3 NAS device (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using port 21/tcp)
HOP RTT      ADDRESS
1   47.82 ms 10.10.14.1
2   47.87 ms 10.129.121.114

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 60.68 seconds
```

We will try to access FTP with the credentials provided.
```
ftp 10.129.121.114
```
![Pasted image 20251120175157](../../IMAGES/Pasted%20image%2020251120175157.png)

Looks like I can't do much with FTP so we are moving on with SSH.
![Pasted image 20251120175536](../../IMAGES/Pasted%20image%2020251120175536.png)
## SSH Enumeration

We will start with ssh-audit finger print
```
git clone https://github.com/jtesta/ssh-audit.git && cd ssh-audit
```
![Pasted image 20251120174721](../../IMAGES/Pasted%20image%2020251120174721.png)

And then we will run the script.
```
./ssh-audit.py 10.129.121.114
```

We will try to access SSH with the credentials provided.
```
ssh ceil@10.129.121.114
```
It denied it because of the publickey. 
![Pasted image 20251120175903](../../IMAGES/Pasted%20image%2020251120175903.png)
We will use password instead.
```
ssh ceil@10.129.121.114 -o PreferredAuthentications=password
```

## Going back
We found out that we can get the key from ftp in directory .ssh

By searching in the internet we have skipped an open port 2121.
we will now use this nmap command:
```
nmap -sV -sC --min-rate=1000 -oN initial_recon 10.129.177.24
```

Now we can access directories and we have found an .ssh directory
![Pasted image 20251120180740](../../IMAGES/Pasted%20image%2020251120180740.png)

We will now get the id_rsa and id_rsa.pub
![Pasted image 20251120180814](../../IMAGES/Pasted%20image%2020251120180814.png)
Now with this key we can access through ssh.
we need first to add permission to the file with:
```
chmod 600 id_rsa
```
![Pasted image 20251120180911](../../IMAGES/Pasted%20image%2020251120180911.png)
we have succesfully connected through ssh.
![Pasted image 20251120181107](../../IMAGES/Pasted%20image%2020251120181107.png)

By browising the directories we have found the flag.
![Pasted image 20251120181312](../../IMAGES/Pasted%20image%2020251120181312.png)
