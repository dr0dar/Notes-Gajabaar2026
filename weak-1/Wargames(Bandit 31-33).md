https://overthewire.org/wargames/bandit/

---
## level 30 ---> level 31

**Level Goal:**
There is a git repository at `ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo` via the port `2220`. The password for the user `bandit30-git` is the same as for the user `bandit30`.
From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

**Solution:**
- `git clone ssh://bandit30-git@51.20.162.29:2220/home/bandit30-git/repo` :  password is `jq9Dfg2rXsfYsWMgFuKlXhphjdH7USgX`.
- It download `repo` directory to our machine.
- `cd repo && ls`: change and list directory got `README.md` file.
- `cat README.md` : Says *just an empty file... muahaha*.
- Lets see pervious commit `git log -p README.md` : There is no pervious commit.
- Lets see for other **branch** `git branch -a` : There are no any other branch.
- Lets see for **tag** `git tag` : Got listed `secret` tag.
- `git show secret` : got password `82NkymblpGBYmIXG6ZQ8YldBYstHpfUf`.

---
## level 31 ---> level 32

**Level Goal:**
There is a git repository at `ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo` via the port `2220`. The password for the user `bandit31-git` is the same as for the user `bandit31`.
From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

**Solution:**
- `git clone ssh://bandit31-git@51.20.162.29:2220/home/bandit31-git/repo` :  password is `82NkymblpGBYmIXG6ZQ8YldBYstHpfUf`.
- It download `repo` directory to our machine.
- `cd repo && ls`: change and list directory got `README.md` file.
- `cat README.md` : Says task is to push file to remote repo. And details are filename: key.txt, Content: 'May I come in?', Branch : master.
- `echo "May I come in?" > key.txt` : creating file with content.
- `git add key.txt` : add key.txt for commit.
- `git commit -m "new commit with key.txt file"`: Create new commit.
- `git push origin master` : push git to remote. And got password `pWuj5jBQ6IgV0NXwiH6g1pXRF8S1YvbT`.

---
## level 32 ---> level 33

**Level Goal:**
After all this `git` stuff, it’s time for another escape. Good luck!

**Solution:**
- `ssh bandit32@51.20.162.29 -p 2220` password is `pWuj5jBQ6IgV0NXwiH6g1pXRF8S1YvbT`
- Here we get Uppercase shell, it run anything we type in uppercase. Because linux in case-sensitive we get error.
- So everything is in uppercase, We have only option of environment variable which are in uppercase and also there is `$0` variable which has reference to the shell.
- `$0` : We get shell
- `ls -l` : we got **uppershell setuid flag binary file of bandit33**
- `whoami` : we are bandit33
- `cat /etc/bandit_pass/bandit33` : got password `u4P2CyPOwPGLe94RdD9Uo2FxFwvnFswM`.

---
[[Wargames(Bandit 21-30)]]