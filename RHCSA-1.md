# Linux RHCSA Notes

## 1. Log into Local & Remote Console

### `ip a`
- **Description:** Shows all IP addresses and network interfaces on the system.
- **Example:**
  ```sh
  ip a
  ```
- **Sample Output:**
  ```
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 ...
      inet 127.0.0.1/8 scope host lo
  2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
      inet 192.168.1.10/24 brd 192.168.1.255 scope global eth0
  ```

### `ssh username@<ipaddr>`
- **Description:** Connects to a remote machine via SSH.
- **Example:**
  ```sh
  ssh user@192.168.1.10
  ```
- **Sample Output:**
  ```
  user@192.168.1.10's password:
  Welcome to Ubuntu 20.04 LTS ...
  ```

### `cat /etc/passwd`
- **Description:** Displays the contents of the `/etc/passwd` file, which contains user account information.
- **Example:**
  ```sh
  cat /etc/passwd
  ```
- **Sample Output:**
  ```
  root:x:0:0:root:/root:/bin/bash
  user:x:1000:1000:User:/home/user:/bin/bash
  ```

### `id username`
- **Description:** Shows user ID (UID), group ID (GID), and group memberships for a user.
- **Example:**
  ```sh
  id root
  ```
- **Sample Output:**
  ```
  uid=0(root) gid=0(root) groups=0(root)
  ```

### `who`
- **Description:** Lists users currently logged in.
- **Example:**
  ```sh
  who
  ```
- **Sample Output:**
  ```
  user   tty1   2026-03-02 08:00
  ```

### `last`
- **Description:** Shows a list of last logged in users and system reboots.
- **Example:**
  ```sh
  last
  ```
- **Sample Output:**
  ```
  user   pts/0   192.168.1.5   Mon Mar  2 08:00   still logged in
  reboot system boot  5.4.0-42-generic Mon Mar  2 07:59
  ```

---

## 2. SSH and Manual Pages

### `ssh -v username@host`
- **Description:** Connects to a remote host with verbose output for debugging.
- **Example:**
  ```sh
  ssh -v alex@localhost
  ```
- **Sample Output:**
  ```
  OpenSSH_8.2p1, OpenSSL 1.1.1g  21 Apr 2020
  debug1: Reading configuration data /etc/ssh/ssh_config
  ...
  ```

### `man <section> <command>`
- **Description:** Shows the manual page for a command. Section 8 is for system administration commands.
- **Example:**
  ```sh
  man 8 reboot
  ```
- **Sample Output:**
  Manual page for system administration command `reboot`.

### `apropos <keyword>`
- **Description:** Searches manual page names and descriptions for a keyword.
- **Example:**
  ```sh
  apropos ssh
  ```
- **Sample Output:**
  ```
  ssh (1) - OpenSSH remote login client
  sshd (8) - OpenSSH SSH daemon
  ```

### `mandb`
- **Description:** Updates the manual page index database (needed if apropos does not work).
- **Example:**
  ```sh
  sudo mandb
  ```
- **Sample Output:**
  Updates manual page index.

---

## 3. File and Directory Operations

### `ls -a <directory>`
- **Description:** Lists all files, including hidden files, in a directory.
- **Example:**
  ```sh
  ls -a /home/bob/data/
  ```
- **Sample Output:**
  ```
  .  ..  file1  .hiddenfile
  ```

### `cd <directory>`
- **Description:** Changes the current directory.
- **Example:**
  ```sh
  cd /home/bob
  ```

### `cp <source> <destination>`
- **Description:** Copies files or directories.
- **Example:**
  ```sh
  cp file1.txt file2.txt
  ```

### `rm <file>`
- **Description:** Removes (deletes) files.
- **Example:**
  ```sh
  rm file1.txt
  ```

### `mv <source> <destination>`
- **Description:** Moves or renames files or directories.
- **Example:**
  ```sh
  mv oldname.txt newname.txt
  ```

### `stat <filename>`
- **Description:** Displays detailed information about a file.
- **Example:**
  ```sh
  stat file1.txt
  ```
- **Sample Output:**
  ```
  File: file1.txt
  Size: 1234       Blocks: 8          IO Block: 4096   regular file
  ...
  ```

### `ln <target> <link>`
- **Description:** Creates a hard link to a file.
- **Example:**
  ```sh
  ln file1.txt link1.txt
  ```

### `ln -s <target> <link>`
- **Description:** Creates a symbolic (soft) link to a file.
- **Example:**
  ```sh
  ln -s file1.txt symlink1.txt
  ```

### `readlink <filename>`
- **Description:** Displays the target of a symbolic link.
- **Example:**
  ```sh
  readlink symlink1.txt
  ```
- **Sample Output:**
  ```
  file1.txt
  ```

---

## 4. Permissions and Ownership

### `ls -l`
- **Description:** Lists files with detailed permissions and ownership.
- **Example:**
  ```sh
  ls -l
  ```
- **Sample Output:**
  ```
  -rw-r--r-- 1 user group 1234 Mar 2 08:00 file1.txt
  ```

### `chgrp <group> <file>`
- **Description:** Changes the group ownership of a file.
- **Example:**
  ```sh
  chgrp staff file1.txt
  ```

### `chown <user> <file>`
- **Description:** Changes the owner of a file.
- **Example:**
  ```sh
  sudo chown bob file1.txt
  ```

### `chown <user>:<group> <file>`
- **Description:** Changes the owner and group of a file.
- **Example:**
  ```sh
  sudo chown bob:staff file1.txt
  ```

### `chmod <mode> <file>`
- **Description:** Changes the permissions of a file.
- **Example:**
  ```sh
  chmod 444 file1.txt
  ```

### Special Permissions
- **SUID:** Set user ID on execution (4xxx)
- **SGID:** Set group ID on execution (2xxx)
- **Sticky Bit:** Restricts file deletion (1xxx)
- **Example:**
  ```sh
  chmod 1777 /tmp
  chmod u+s,g+s,o+t /home/bob/datadir
  ```

---

## 5. Finding Files

### `find` command
- **Description:** Searches for files and directories based on criteria.
- **Examples:**
  ```sh
  find /home/bob/data/ -name "file*"
  find /var/log/ -perm -g=w -not -perm /o=rw
  find /usr -type f -mmin -120
  find /usr -type f -size +5M -size -10M
  ```
- **Sample Output:**
  ```
  /home/bob/data/file1
  /var/log/syslog
  /usr/bin/largefile
  ```

---

## 6. Comparing and Manipulating File Content

### `cat` and `tac`
- **Description:** `cat` displays file content from top to bottom; `tac` displays from bottom to top.
- **Example:**
  ```sh
  cat file.txt
  tac file.txt
  ```

### `head` and `tail`
- **Description:** `head` shows the first lines; `tail` shows the last lines of a file.
- **Example:**
  ```sh
  head -n 20 var.log
  tail -n 20 var.log
  ```

### `sed` (Stream Editor)
- **Description:** Edits text in a stream or file.
- **Example:**
  ```sh
  sed -i 's/canda/canada/g' filename.txt
  ```
- **Flags:**
  - `g`: global (all matches)
  - `s`: substitute/search
  - `-i`: edit file in place

### `cut`
- **Description:** Cuts sections from each line of files.
- **Example:**
  ```sh
  cut -d ' ' -f 1 filename.txt
  ```
- **Flags:**
  - `-d`: delimiter
  - `-f`: field

### `sort` and `uniq`
- **Description:** `sort` sorts lines; `uniq` removes adjacent duplicates (sort first for all duplicates).
- **Example:**
  ```sh
  sort filename.txt | uniq
  ```

### `diff` and `sdiff`
- **Description:** Compares files line by line.
- **Example:**
  ```sh
  diff file1 file2
  diff -c file1 file2
  diff -y file1 file2
  sdiff file1 file2
  ```

---

## 7. Viewing Files (Pagers)

### `less` and `more`
- **Description:** View file content one page at a time.
- **Example:**
  ```sh
  less /var/log/dnf.log
  more /var/log/dnf.log
  ```

### `vim`
- **Description:** Opens the Vim text editor.
- **Example:**
  ```sh
  vim filename.txt
  ```

---

## 8. Searching Text (grep)

### `grep`
- **Description:** Searches for patterns in files.
- **Examples:**
  ```sh
  grep 'centos' /etc/os-release
  grep -i 'centos' /etc/os-release
  grep -r 'CentOs' /etc/
  grep -v 'pattern' file.txt   # invert match
  grep -w 'word' file.txt      # match whole word
  grep -o 'pattern' file.txt   # only matching part
  ```
- **Flags:**
  - `-i`: ignore case
  - `-r`: recursive
  - `-v`: invert match
  - `-w`: match whole word
  - `-o`: only matching part

---

## 9. Regular Expressions (regex)

- **Operators:** `^` (start), `$` (end), `.`, `*`, `+`, `{}`, `?`, `|`, `[]`, `()`, `[^]`
- **Examples:**
  ```sh
  grep '^ERROR' filename
  grep 'logout$' filename
  grep -r '/.*/' /etc/
  ```

---

## 10. Archiving and Compression

### `tar` (Tape Archive)
- **Description:** Archives and extracts files.
- **Examples:**
  ```sh
  tar -tf archive.tar           # List files in archive
  tar cf archive.tar file1      # Create archive
  tar rf archive.tar file2      # Append file
  tar xf archive.tar            # Extract archive
  tar xf archive.tar -C /tmp/   # Extract to directory
  tar czf archive.tar.gz file1  # Create gzipped archive
  tar cjf archive.tar.bz2 file1 # Create bzip2 archive
  tar cJf archive.tar.xz file1  # Create xz archive
  ```
- **Flags:**
  - `-t`: list
  - `-c`: create
  - `-x`: extract
  - `-f`: file
  - `-z`: gzip
  - `-j`: bzip2
  - `-J`: xz

### `gzip`, `bzip2`, `xz`
- **Description:** Compress files.
- **Examples:**
  ```sh
  gzip file1
  bzip2 file2
  xz file3
  ```

### `gunzip`, `bunzip2`, `unxz`
- **Description:** Decompress files.
- **Examples:**
  ```sh
  gunzip file1.gz
  bunzip2 file2.bz2
  unxz file3.xz
  ```

### `zip` and `unzip`
- **Description:** Create and extract zip archives.
- **Examples:**
  ```sh
  zip -r archive.zip /dir
  unzip archive.zip
  ```

### `star`
- **Description:** Another archiving tool.
- **Examples:**
  ```sh
  star -cv file=/home/user/vijay/archive.star file1
  star -tv file=/home/user/vijay/archive.star
  star -xv file=/home/user/vijay/archive.star
  ```
- **Flags:**
  - `-c`: create
  - `-t`: list
  - `-x`: extract
  - `-v`: verbose
  - `-z`: compress in gzip

---

## 11. Redirecting Output and Piping

### Output Redirection
- `>` : overwrite output
- `>>` : append output
- `2>` : redirect stderr
- `1>` or `>` : redirect stdout
- `<` : input redirection
- `2>&1` : combine stderr and stdout

### Examples
```sh
grep -r '^The' /etc/ 1>output.txt 2>errors.txt    # overwrite
grep -r '^The' /etc/ 1>>output.txt 2>>errors.txt  # append
grep -r '^The' /etc/ > alloutput.txt 2>&1         # all output
```

### Heredoc and Herestring
- **Heredoc Example:**
  ```sh
  sort <<EOF
  65
  3
  7889
  EOF
  ```
- **Herestring Example:**
  ```sh
  bc <<<"1+2+3+5"
  ```

### Piping
- **Description:** Sends output of one command as input to another.
- **Example:**
  ```sh
  ls | grep txt
  ```

---

## 12. File Transfer and Backup

### `rsync`
- **Description:** Synchronizes files and directories between local and remote systems.
- **Examples:**
  ```sh
  rsync -a Pictures/ vijay@9.9.9.9:/home/vijay/Pictures/   # local to remote
  rsync -a vijay@9.9.9.9:/home/vijay/Pictures/ Pictures/   # remote to local
  ```

### `dd`
- **Description:** Disk imaging and cloning tool.
- **Examples:**
  ```sh
  sudo dd if=/dev/vda of=diskimage.raw bs=1M status=progress   # disk to image
  sudo dd if=diskimage.raw of=/dev/vda bs=1M status=progress   # image to disk
  ```

### `scp`
- **Description:** Securely copies files between hosts over SSH.
- **Examples:**
  ```sh
  scp vijay@192.168.1.2:/home/vijay/myfile.tgz /home/vijay/myfile.tgz   # remote to local
  scp /home/vijay/myfile.tgz vijay@192.168.1.2:/home/vijay/myfile.tgz   # local to remote
  scp vijay@192.168.1.2:/home/vijay/myfile.tgz vijay@192.168.1.34:/home/vijay/myfile.tgz   # remote to remote
  ```

### `sftp`
- **Description:** Secure File Transfer Protocol for interactive file transfer over SSH.
- **Examples:**
  ```sh
  sftp vijay@192.168.1.2
  # At the sftp prompt:
  sftp> get image.jpg        # remote to local
  sftp> get -r images/       # remote directory to local
  sftp> put archive.tar.gz   # local to remote
  sftp> put -r images/       # local directory to remote
  ```

---

# Questions and Answers

---

**Q: The ssh command has an option to display its version number. Use man to find out what is the correct command line option.**
- **A:** `ssh -V`

**Q: Find out using which command you can change the static hostname of your Linux system?**
- **A:** `hostnamectl`

**Q: If the apropos command does not work because your manual pages are not indexed, what command you can use to manually refresh these?**
- **A:** `mandb`

**Q: You are trying to use ssh alex@localhost to log in through SSH. Your connection is refused. ssh has a command line option to show you the verbose output. What is the correct option for that?**
- **A:** `ssh -v alex@localhost`

**Q: You type host in the terminal. What keys do you press to see some suggestions about what you can type here?**
- **A:** Press `TAB TAB`

**Q: What section of the manual pages deals with System administration commands?**
- **A:** Section 8

**Q: How many hidden files are there in /home/bob/data/ directory?**
- **A:** Use `ls -a /home/bob/data/` to list all files, including hidden ones (those starting with a dot).

**Q: We are trying to run apropos ssh command to get some details about the commands related to ssh but we are getting this error: ssh: nothing appropriate. Look into the issue and fix the same to make apropos ssh command work.**
- **A:** Run `sudo mandb` to update the manual page index.

**Q: Using apropos command, find out the configuration file for NFS mounts and save its name in /home/bob/nfs file.**
- **A:**
  1. Run `apropos "NFS mounts"` (output: `nfsmount.conf (5) - Configuration file for NFS mounts`)
  2. Save the name to file: `echo nfsmount.conf > /home/bob/nfs`

**Q: What command would find files and directories modified in the last 5 minutes in /dev directory?**
- **A:** `find /dev/ -mmin -5`

**Q: What command removes the write permission for the group from a file?**
- **A:** `chmod g-w somefile`

**Q: Find files/directories under /var/log/ directory that the group can write to, but others cannot read or write to it. Save the list of the files/directories (with complete parent path) in /home/bob/data.txt file.**
- **A:** `sudo find /var/log/ -perm -g=w -not -perm /o=rw > /home/bob/data.txt`

**Q: Find our secret file under /home/bob. You can either look for a file that is exactly 213 kilobytes or a file that has permission 402 in octal. Save the name (including the parent directory path) of this file in /home/bob/secfile.txt file.**
- **A:** `find /home/bob -size 213k -o -perm 402 > /home/bob/secfile.txt`

**Q: Add the permissions for setuid, setgid and sticky bit on /home/bob/datadir directory.**
- **A:** `chmod u+s,g+s,o+t /home/bob/datadir` or `chmod 7755 datadir/`

**Q: Find all directories named pets in /var/directory and save the output (along with directory path) in /home/bob/pets.txt file.**
- **A:** `sudo find /var/ -type d -name pets > /home/bob/pets.txt`

**Q: Find all the files which have been modified in the last 2 hours in /usr directory. How many such files did you find?**
- **A:** `sudo find /usr -type f -mmin -120 | wc -l`

**Q: Find all files between 5mb and 10mb in the /usr directory and save the output (along with parent path) in /home/bob/size.txt file.**
- **A:** `sudo find /usr -type f -size +5M -size -10M > /home/bob/size.txt`

**Q: Create a directory named LFCS under bob's home directory and update its user owner permissions to only x (execute), and group and others should not have any permissions.**
- **A:**
  1. `sudo mkdir /home/bob/LFCS`
  2. `sudo chmod 0100 /home/bob/LFCS`

---