https://overthewire.org/wargames/bandit/
---
## lever 0

**Level Goal:**
Login into **bandit.labs.overthewire.org, on port 2220** through ssh.
The username is **bandit0** and the password is **bandit0**.

**Solution:**
- `ping bandit.labs.overthewire.org` : PING bandit.labs.overthewire.org (51.20.162.29)
- `ssh bandit0@51.20.162.29 -p 2220`

---
## level 0 --> level 1

**Level Goal:**
Get password  located in `readme` file in home directory of bandit0 user and use this password to login into bandit1 user.

**Solution:**
- `ssh bandit0@51.20.162.29 -p 2220` : password is bandit0
- `ls` : got listed readme file
- `cat readme` : got password `6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR`
- `ssh bandit1@51.20.162.29 -p 2220` : password is `6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR`

---
## level 1 --> level 2

**Level Goal:**
Get next level password  located in `-` file in home directory of bandit1 user.

**Note:** In linux and unix-like os `-` refers to the indicator for a command option or arguments. That\`s why for dealing with the `-` files, best  approch should be use the path prefixing approach (`./-test`). Eg: `cat ./-`.

**Solution:**
- `ssh bandit1@51.20.162.29 -p 2220` : password is `6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR`
- `ls` : got listed `-` file
- `cat < -`  or `cat ./-` : got password `PK8fYLZg2hnHSz83plBL1iEPKdD3QToB`

---
## level 2 ---> level 3

**Level Goal:**
The password for the next level is stored in a file called `--spaces in this filename--` located in the home directory.

**Solution:**
- `ssh bandit2@51.20.162.29 -p 2220` : password is `PK8fYLZg2hnHSz83plBL1iEPKdD3QToB`
- `ls` : got listed `--spaces in this filename--` file
- `cat './--spaces in this filename--'`  : got password `7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME`

---
## level 3 ---> level 4

**Level Goal:**
The password for the next level is stored in a hidden file in the **inhere** directory.

**Solution:**
- `ssh bandit3@51.20.162.29 -p 2220` : password is `7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME`
- `ls` : got listed inhere directory
- `cd inhere` : inside inhere
- `ls -a` : got all files also hidden.
- `cat ...Hiding-From-You`  : got password `xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq`

---
## level 4 ---> level 5

**Level Goal:**
The password for the next level is stored in the only human-readable file in the **inhere** directory.

**Solution:**
- `ssh bandit4@51.20.162.29 -p 2220` : password is `xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq`
- `ls` : got listed inhere directory
- `cd inhere` : inside inhere
- `ls` : got multiple files.
- `cat ./-file0{0,1,2,3,4,5,6,7,8,9}`  : got password `6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG`

---
## level 5 ---> level 6

**Level Goal:**
The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:
- human-readable
- 1033 bytes in size
- not executable

**Solution:**
- `ssh bandit5@51.20.162.29 -p 2220` : password is `6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG`
- `ls` : got listed inhere directory
- `cd inhere` : inside inhere
- `ls` : got multiple directories .
- `find . -size 1033c` : got required file `./maybehere07/.file2`
- `cat ./maybehere07/.file2`  : got password `pXa26xhMWaC2SvDotA4r9EgZkulOeSBW`

---
## level 6 ---> level 7

**Level Goal:**
The password for the next level is stored **somewhere on the server** and has all of the following properties:
- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

**Solution:**
- `ssh bandit6@51.20.162.29 -p 2220` : password is `pXa26xhMWaC2SvDotA4r9EgZkulOeSBW`
- `find / -user bandit7 -group bandit6 -size 33c -type f 2>/dev/null` : got required file `/var/lib/dpkg/info/bandit7.password`
- `cat /var/lib/dpkg/info/bandit7.password`  : got password `Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3`

---
## level 7 ---> level 8

**Level Goal:**
The password for the next level is stored in the file **data.txt** next to the word **millionth**

**Solution:**
- `ssh bandit7@51.20.162.29 -p 2220` : password is `Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3`
- `ls` : got required file `data.txt`
-  `grep "millionth" data.txt`: got password `VR1ljMayciFxbnUokuQmJFw6QC9VKtub`

---
## level 8 ---> level 9

**Level Goal:**
The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

**Solution:**
- `ssh bandit8@51.20.162.29 -p 2220` : password is `VR1ljMayciFxbnUokuQmJFw6QC9VKtub`
- `ls` : got required file `data.txt`
- `cat data.txt | sort | uniq -u` : got password `EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl`

---
## level 9 ---> level 10

**Level Goal:**
The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

**Solution:**
- `ssh bandit9@51.20.162.29 -p 2220` : password is `EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl`
- `ls` : got required file `data.txt`
- `strings data.txt` : got password `B0s2khmbT9u0geKuOoVGW3JZKhndE3BG`

---
## level 10 ---> level 11

**Level Goal:**
The password for the next level is stored in the file **data.txt**, which contains base64 encoded data.

**Solution:**
- `ssh bandit10@51.20.162.29 -p 2220` : password is `B0s2khmbT9u0geKuOoVGW3JZKhndE3BG`
- `ls` : got required file `data.txt`
- `base64 -d data.txt` : got password `pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro`

---
## level 11 ---> level 12

**Level Goal:**
The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.

**Solution:**
- `ssh bandit11@51.20.162.29 -p 2220` : password is `pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro`
- `ls` : got required file `data.txt`
- `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'` : got password `GROozWPO8QyN0mGrjUkID0WCYkZiQxrN`

---

