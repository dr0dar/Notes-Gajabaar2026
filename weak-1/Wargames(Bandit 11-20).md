https://overthewire.org/wargames/bandit/

---
## level 11 ---> level 12

**Level Goal:**
The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.

**Note:** `tr` means translation of character, and it takes two argument first is select which character and second is change to which character

**Solution:**
- `ssh bandit11@51.20.162.29 -p 2220` : password is `pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro`
- `ls` : got required file `data.txt`
- `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'` : got password `GROozWPO8QyN0mGrjUkID0WCYkZiQxrN`

---
## level 12 ---> level 13

**Level Goal:**
The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed.

**Notes:** `xxd` show binary to hexdump and reverse also and `file` gives us file type.

**Solution:**
- `ssh bandit12@51.20.162.29 -p 2220` : password is `GROozWPO8QyN0mGrjUkID0WCYkZiQxrN`
- `ls` : got required file `data.txt`
- `mkdir /tmp/hextopwd` : create new directory in /tmp
- `cd /tmp/hextopwd` :  change directory to hextopwd
- `cp ~/data.txt .` : copy hexdump file to current directory
- `mv data.txt hexdump.txt` : rename file.
- `xxd -r hexfum.txt a` : reverse hexdump to original file.

- `file a` :  Gives us its  file type (**gzip**)
- `mv a a.gz` : convert to gzip compress file.
- `gzip -d a.gz` : decompress gzip.

- `file a` : Gives us **bzip2** compressed.
- `mv a a.bz2` : convert to bzip2 compress file.
- `bzip2 -d a.bz2`  or `bunzip2 a.bz2` : decompress bzip2.

- `file a` :  Gives us **gzip** compressed.
- `mv a a.gz` : convert to gzip compress file.
- `gzip -d a.gz` : decompress gzip.

- `file a` :  Gives us **tar** archive.
- `mv a a.tar` : convert to tar archive.
- `tar -xf a.tar` : untar tar archive.
- `ls` : get **data5.bin** file.

- `file data5.bin` :  Gives us **tar** archive.
- `mv data5.bin data5.tar` : convert to tar archive.
- `tar -xf data5.tar` : untar tar archive.
- `ls` : get **data6.bin** file.

- `file data6.bin` : Gives us **bzip2** compressed.
- `mv data6.bin data6.bz2` : convert to bzip2 compress file.
- `bzip2 -d data6.bz2`  or `bunzip2 data6.bz2` : decompress bzip2.

- `file data6` :  Gives us **tar** archive.
- `mv data6 data6.tar` : convert to tar archive.
- `tar -xf data6.tar` : untar tar archive.
- `ls` : get **data8.bin** file.

- `file data8.bin` :  Gives us **gzip** compressed.
- `mv data8.bin data8.gz` : convert to gzip compress file.
- `gzip -d data8.gz` : decompress gzip.

- `file data8` : finally got ASCII text
- `cat data8` : got password `qQYQiHOBPR8zR61qxYqX45quvihF2uzk`

---
## level 13 ---> level 14

**Level Goal:**
The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level.

**Solution:**
- `ssh bandit13@51.20.162.29 -p 2220` : password is `qQYQiHOBPR8zR61qxYqX45quvihF2uzk`
- `ls` : got **sshkey.private** file
- Back to local machine and download sshkey.private to local machine using `scp -P 2220 bandit13@51.20.162.29:~/sshkey.private .`
- Look at the permission of sshkey.private `ls -l`
- Add execution permission to user `chmod 700 sshkey.private`
- Login to bandit14 `ssh -i sshkey.private bandit14@51.20.162.29 -p 2220`
- `cat /etc/bandit_pass/bandit14` : got password `aaWecNkG4FhxJQxz07uiwzVP6bJiYS65`

---
## level 14 ---> level 15

**Level Goal:**
The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

**Solution:**
- `ssh bandit14@51.20.162.29 -p 2220` : password is `aaWecNkG4FhxJQxz07uiwzVP6bJiYS65`
- `nc 127.0.0.1 30000 < /etc/bandit_pass/bandit14` : got password `pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7`

---
## level 15 ---> level16

**Level Goal:**
The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL/TLS encryption.

**Note:** `nc` does not support SSL/TLS encrtyption, So we need to use `ncat with --ssl`.

**Solution:**
- `ssh bandit15@51.20.162.29 -p 2220` : password is `pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7`
- `echo "pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7" | ncat 127.0.0.1 30001 --ssl` : got password `kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V`

---
## level 16 ---> level17

**Level Goal:**
The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

**Solution:**
- `ssh bandit16@51.20.162.29 -p 2220` : password is `kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V`
- `nc -z 127.0.0.1 31000-32000` or `nmap 127.0.0.1 -p 31000-32000` : got open ports `31046, 31518, 31691, 31790, 31960`
- `nmap --script ssl-cert 127.0.0.1 -p 31046,31518,31691,31790,31960` : got port which speck SSL/TSL = `31518, 31790`
- `echo "kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V" | ncat 127.0.0.1 31790 --ssl` : got next level ssh-private-key ;
```text
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----

```
- `ssh -i sshkeybandit17.private bandit17@51.20.162.29 -p 2220` : login to bandit17 system.
- `cat /etc/bandit_pass/bandit17` : got next level(bandit17) password also `pWXMAZoxGC8JmDMfmT5MGEsobMM3vnj2`.

---
## level 17 ---> level18

**Level Goal:**
There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**.

**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**

**Note:** `diff` command is used to find difference between file. It search difference line by line and provide `a`(add), `d`(delete), `c`(change) instruction with line number \[Eg: 4c4 where first line number is from first file and second line number is from second file.] to make both file same. 

**Solution:**
- `ssh bandit17@51.20.162.29 -p 2220` : password is `pWXMAZoxGC8JmDMfmT5MGEsobMM3vnj2`
- `diff passwords.old passwords.new` :  got next level password `OQxXZjELndr90zuhOTDYBEomI0SZITXI`

---
## level 18 ---> level19

**Level Goal:**
The password for the next level is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.

**Solution:**
- `ssh bandit18@51.20.162.29 -p 2220 'cat ~/readme'`  : password is `OQxXZjELndr90zuhOTDYBEomI0SZITXI`
-  Above command give use next level password `KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI`

---
## level 19 ---> level 20

**Level Goal:**
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

**Note:**
- In linux and unix system every user is assigned userid (uid) and groupid (gid). which are used by system to know which file has access to which uid and gid.
- Sometime user need other user permissions for that reason users are frovided with `setuid` and `setgid` flags  and it is a **special file permission flag** set on an executable file by an administrator.
- When a program has the `setuid` bit enabled, anyone who executes that program will **temporarily inherit the permissions of the file's owner**.
- The `setuid` and `setgid` flags have an effect only on binary executable files and not on scripts (e.g., Bash, Perl, Python).
- `id` and `id <user>` this command are used to see uid and guid of users.

**Solution:**
- `ssh bandit19@51.20.162.29 -p 2220`  : password is `KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI`
- `ls` : got file `bandit20-do`
- `./bandit20-do` : got info on who to use this file
- `./bandit20-do cat /etc/bandit_pass/bandit20` : got next level password `4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA`.

---
## level 20 ---> level 21

**Level Goal:**
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

**NOTE:** Try connecting to your own network daemon to see if it works as you think

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

[[Wargames(Bandit 0-10)]]