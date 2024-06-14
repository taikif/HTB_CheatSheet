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
  
## Privilege Escalation
・sudo -l <br>
  ー 1. root権限で実行できるコトを探す。 <br>
  ー 2. 実行できるコトを悪用する。 <br>
  　ー 2-1. Pythonスクリプトをローカルのshを参照して実行している場合に、ディレクトリを移動して独自のshを実行するようにする等。 <br>
   　ー 2-2. hogehoge.shの中身を#!/bin/bash \n chmod u+s /bin/bashとする。それを実行させる。 <br>
    　ー 2-3. bash -p 実行。→ 特権昇格。 <br>      
・linPEAS: sudo -lしてもいい感じの出てこないときに使う。 <br>
  ー 1. linPEASでUnknown SUID binaryを特定。 <br>
  ー 2. そのコマンドのバージョンを確認。脆弱性を探す。 <br>
  ー 3. 脆弱性を突いて特権昇格。 <br>
https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS?source=post_page-----7b26a767293a--------------------------------

# Windows
