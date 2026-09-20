When we provide something with command this are arguments. Eg: `ls -l Downloads` here `-l` and `Downloads` are arguments.

----
### Globs
- **\* (zero or more of anything):** `rm \*.py`  = * glob search for anything followed by .py like new.py, test.py, 1.py and it remove all file.
- **{} (expand a comma-seperated list of patterns into multiple arguments):** `mkdir {new,test,first}.py` = it expands into `mkdir new.py test.py first.py`

`convert image.{png,jpg}` , `mv *{.py,.sh} folder`

---

`|` pipe run commands in **parellel**.

---
### Signals
Processes are work with signals when we type Ctrl+C in terminal, shell send signal to the running process to stop it. Signals are _software interrupts_.
**Common signals:**

| Command            | Signals | Description         |
| ------------------ | ------- | ------------------- |
| Ctrl+C             | SIGINT  | Terminal Interrupt  |
| Ctrl+\             | SIGQUIT | Terminal Quit       |
| kill -TERM  \<pid> | SIGTERM | Termination request |
| Ctrl+Z             | SIGTSTP | Termianl stop       |
- We can continue the pause job in foreground or in background using `fg` or `bg`, respectively. `fg %1`
- `jobs` command list all unfinished jobs in current terminal session.
---
### Remote Machine
**ssh:**
- Connection `ssh <user>@<ip-address>`
- Command run `ssh <user>@<ip-address> <commands>`. If add command after `|` it run in our machine with the output of remote machine output 
- ssh-keygen `ssh-keygen -a 100 -t ed25519 -f ~/.ssh/id_ed25519`. It generates private key `id_rsa` and public key `id_ed25519` in `~/.ssh/` .
- In remote server we need to add our public key into `~/.ssh/authorized_keys` for login using ssh keys. `cat .ssh/id_ed25519.pub | ssh <user>@<ip-address> 'cat >> ~./ssh/authorized_keys'`
- File transfer in local to remote machine `scp path/to/local_file <user>@<ip_address>:path/to/remote_file`
---
### Terminal Multiplexer

**`tmux`:**

`<c-b>` means press Ctrl+B and release buttons 
`<c-b> ?` show all key bindings
[Ream more](https://linuxcommand.org/lc3_adv_termmux.php)

session:
- `tmux ls` list all session 
- `tmux` starts a new session window.
- `tmux new -s <name>` start new session  window with name
- `Ctrl+D` Delete and exit session window
- `<c-b> d` detach current session 
- `tmux a` attach detached session 
windows:
- `<c-b> c` for create new window on current session.
- `<c-b> n` for chage  n^th window on current session.
- `<c-b> p` pervious window and `<c-b> n` next window.
- `<c-b> ,` rename window
- `<c-b> w` list windows of current session.
panels:
- `<c-b> "` split current panel horizontally
- `<c-b> %` split vertically
- `<c-b> <any-arrow>` for change working  panel
- `<c-b> z` maximize current panel
- `<c-b> <space>` cycle panels in arrange( first vertically then horizontally etc.)
- `<c-b> [` Scroll bar in terminal press `q` for exit 

---
### Dotfiles
We can customize our shell through dotfiles. Files which names begin with `.`, `~/.bashrc` .

For `bash`, editing `.bashrc` or `. bash_profile` will work in most systems.

dotfiles for some tools are:
- `bash` - `~/.bashrc`, `~/.bash_profile`
- `git` - `~/.gitconfig`
- `vim` - `~/.vimrc` and the `~/.vim` folder
- `ssh` -  `~/.ssh/config`
- `tmux` - `~/.tmux.config`
---
### Alias
A shell alias is a **short form** for another **commands**
We can create our own commads aliases using `alias` shell buite-in.

`alias <alias_name>="<long_command>"` no space around `=`

---
For reverse history search we can press `Ctrl+R` and start typing to search

---
 You can search for installation process for any command through  [command-not-found.com](https://command-not-found.com/)
 
---

[[Some Fundamentals]]

