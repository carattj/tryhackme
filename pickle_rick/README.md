# https://tryhackme.com/room/picklerick

I connect to the web application via browser, and I look at the html content putting 'view-source:' before the url.
Inside the html body, I find a username:

Username: R1ckRul3s

I analyze network requests and file sources without finding anything interesting. Therefore, I use gobuster to identify hidden directories and files. As wordlist, I use one of the many provided in Kali Linux. I also specify the extensions to look for through the -x flag.

>> gobuster dir -u <ip_address> -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x php,sh,js,txt,cgi,html,css,py

I discover many pages, but the ones I care the most are /robots.txt and /login.php. In /robots.txt, I find the following strange written:

Wubbalubbadubdub

In the /login.php page, there is a login form. I have a username and a strange written that could be used as a password...it works!
I navigate through the pages. There is only one interesting thing: a text input to execute commands directly to the target machine. I write a simple:

>> ls -la
total 40
drwxr-xr-x 3 root   root   4096 Feb 10  2019 .
drwxr-xr-x 3 root   root   4096 Feb 10  2019 ..
-rwxr-xr-x 1 ubuntu ubuntu   17 Feb 10  2019 Sup3rS3cretPickl3Ingred.txt
drwxrwxr-x 2 ubuntu ubuntu 4096 Feb 10  2019 assets
-rwxr-xr-x 1 ubuntu ubuntu   54 Feb 10  2019 clue.txt
-rwxr-xr-x 1 ubuntu ubuntu 1105 Feb 10  2019 denied.php
-rwxrwxrwx 1 ubuntu ubuntu 1062 Feb 10  2019 index.html
-rwxr-xr-x 1 ubuntu ubuntu 1438 Feb 10  2019 login.php
-rwxr-xr-x 1 ubuntu ubuntu 2044 Feb 10  2019 portal.php
-rwxr-xr-x 1 ubuntu ubuntu   17 Feb 10  2019 robots.txt

I search for /Sup3rS3cretPickl3Ingred.txt, and I find the first flag: mr. meeseek hair.

Commands that cannot be executed are suggesting me to navigate through the filesystem. I cannot do it directly, since some of them are blocked, therefore I see if python run on this machine. Python doesn't work, but python3 does! Therefore, I can upload a python3 reverse shell to the target machine and execute it. Here (https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet), I find a ready python3 reverse shell.
I customize the script to use my IP address with a known port, and I put my machine on listening for uncoming communications:

nc -lnvp 9999

I'm on the system! I look around and find the second flag in /home, and the third in /home/rick: respectively 'fleeb juice' and '1 jerry tear'.