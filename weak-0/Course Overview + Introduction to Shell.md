**Note :**
When running any command, shell looks for `$PATH` variable for search the path of program of this command.

That means in Linux for run program/script we just have to give its path.

Eg:- If we have bash script in current directory we run by `./script.sh` and `.` means current directory. Here we are giving only path for run program likely commands are also a program and when we type `nmap` shell first looks for `$PATH` variable which have bunch of path if `nmap` is in those paths it run, otherwise it will not run (command not found).

We can also  add some path in this variable permanently through `.bashrc` in bash shell (**Bourne Again Shell**). `export $PATH:/home`

---

[[Some Fundamentals]]

