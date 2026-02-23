# Day 10 – File Permissions & File Operations Challenge

## Task: Completed

Master file permissions and basic file operations in Linux.

- Create and read files using `touch`, `cat`, `vim`
- Understand and modify permissions using `chmod`

---

## Challenge Tasks

### Task 1: Create Files (10 minutes)

1. Create empty file `devops.txt` using `touch`
2. Create `notes.txt` with some content using `cat` or `echo`
3. Create `script.sh` using `vim` with content: `echo "Hello DevOps"`

**Verify:** `ls -l` to see permissions

Answer:

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ touch devops.txt
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls
    devops.txt

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ echo "Hello i am learning devops with TWS " > notes.txt

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls
    devops.txt  notes.txt

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ echo "One of the best experience " > notes.txt
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ echo "Yes love the learning" > notes.txt
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls
    devops.txt  notes.txt

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ cat notes.txt
    Yes love the learning

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ echo "One of the best experience " >> notes.txt
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ cat notes.txt
    Yes love the learning
    One of the best experience

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ echo "Hello i am learning devops with TWS " >> notes.txt

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ cat notes.txt
    Yes love the learning
    One of the best experience
    Hello i am learning devops with TWS

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls
    devops.txt  notes.txt

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ vim script.sh
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls
    devops.txt  notes.txt  script.sh
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ cat script.sh
    echo "Hello DevOps"

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls -l
    total 8
    -rw-rw-r-- 1 ubuntu ubuntu  0 Feb 23 07:55 devops.txt
    -rw-rw-r-- 1 ubuntu ubuntu 87 Feb 23 08:10 notes.txt
    -rw-rw-r-- 1 ubuntu ubuntu 20 Feb 23 08:11 script.sh

---

### Task 2: Read Files (10 minutes)

1. Read `notes.txt` using `cat`
2. View `script.sh` in vim read-only mode
3. Display first 5 lines of `/etc/passwd` using `head`
4. Display last 5 lines of `/etc/passwd` using `tail`

Answer:

      ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ cat /etc/passwd | head
      root:x:0:0:root:/root:/bin/bash
      daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
      bin:x:2:2:bin:/bin:/usr/sbin/nologin
      sys:x:3:3:sys:/dev:/usr/sbin/nologin
      sync:x:4:65534:sync:/bin:/bin/sync
      games:x:5:60:games:/usr/games:/usr/sbin/nologin
      man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
      lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
      mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
      news:x:9:9:news:/var/spool/news:/usr/sbin/nologin

      ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ cat /etc/passwd | tail
      fwupd-refresh:x:990:990:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
      polkitd:x:989:989:User for polkitd:/:/usr/sbin/nologin
      ec2-instance-connect:x:109:65534::/nonexistent:/usr/sbin/nologin
      _chrony:x:110:112:Chrony daemon,,,:/var/lib/chrony:/usr/sbin/nologin
      ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
      dnsmasq:x:999:65534:dnsmasq:/var/lib/misc:/usr/sbin/nologin
      takyo:x:1001:1001::/home/takyo:/bin/bash
      professor:x:1003:1003::/home/professor:/bin/bash
      berlinn:x:1004:1004::/home/berlinn:/bin/bash
      nairobi:x:1005:1007::/home/nairobi:/bin/bash
      ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$

---

### Task 3: Understand Permissions (10 minutes)

Format: `rwxrwxrwx` (owner-group-others)

- `r` = read (4), `w` = write (2), `x` = execute (1)

Check your files: `ls -l devops.txt notes.txt script.sh`

Answer: What are current permissions? Who can read/write/execute?

This are my current permissions: -rw-rw-r-- which is 664

Owner and Group are the Ubuntu and ubuntu and can read and write but not execute the files.

All file

ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls -l
total 8
-rw-rw-r-- 1 ubuntu ubuntu 0 Feb 23 07:55 devops.txt
-rw-rw-r-- 1 ubuntu ubuntu 87 Feb 23 08:10 notes.txt
-rw-rw-r-- 1 ubuntu ubuntu 20 Feb 23 08:11 script.sh

---

### Task 4: Modify Permissions (20 minutes)

1. Make `script.sh` executable → run it with `./script.sh`

Answer:

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ sudo ./script.sh
    sudo: ./script.sh: command not found
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ sudo chmod 764 script.sh
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ./script.sh
    Hello DevOps

2. Set `devops.txt` to read-only (remove write for all)

Answer:

      ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ sudo chmod 444 devops.txt
      ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls -l
      total 8
      -r--r--r-- 1 ubuntu ubuntu  0 Feb 23 07:55 devops.txt

3. Set `notes.txt` to `640` (owner: rw, group: r, others: none)

Answer:

      ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ sudo chmod 640 notes.txt
      ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls -l
      total 8
      -r--r--r-- 1 ubuntu ubuntu  0 Feb 23 07:55 devops.txt
      -rw-r----- 1 ubuntu ubuntu 87 Feb 23 08:10 notes.txt
      -rwxrw-r-- 1 ubuntu ubuntu 20 Feb 23 08:11 script.sh

4. Create directory `project/` with permissions `755`

Answer:

      ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls -l
      total 12
      -r--r--r-- 1 ubuntu ubuntu    0 Feb 23 07:55 devops.txt
      -rw-r----- 1 ubuntu ubuntu   87 Feb 23 08:10 notes.txt
      drwxrwxr-x 2 ubuntu ubuntu 4096 Feb 23 08:39 project
      -rwxrw-r-- 1 ubuntu ubuntu   20 Feb 23 08:11 script.sh

      ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ sudo chmod 755 project/

      ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls -l
      total 12
      -r--r--r-- 1 ubuntu ubuntu    0 Feb 23 07:55 devops.txt
      -rw-r----- 1 ubuntu ubuntu   87 Feb 23 08:10 notes.txt
      drwxr-xr-x 2 ubuntu ubuntu 4096 Feb 23 08:39 project
      -rwxrw-r-- 1 ubuntu ubuntu   20 Feb 23 08:11 script.sh

**Verify:** `ls -l` after each change

Answer:

---

### Task 5: Test Permissions (10 minutes)

1. Try writing to a read-only file - what happens?

Answer: It shows permission denied.

ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ echo "hello" > devops.txt
-bash: devops.txt: Permission denied

2. Try executing a file without execute permission

Answer:

    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ./script.sh
    -bash: ./script.sh: Permission denied
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls -l
    total 16
    -r--r--r-- 1 ubuntu ubuntu   17 Feb 23 08:43 devops.txt
    -rw-r----- 1 ubuntu ubuntu   87 Feb 23 08:10 notes.txt
    drwxr-xr-x 2 ubuntu ubuntu 4096 Feb 23 08:39 project
    -rw-rw-rw- 1 ubuntu ubuntu   20 Feb 23 08:11 script.sh
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ sudo chmod 766 script.sh
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ./script.sh
    Hello DevOps
    ubuntu@ip-172-31-18-13:~/Devops_challenge/day10$ ls -l
    total 16
    -r--r--r-- 1 ubuntu ubuntu   17 Feb 23 08:43 devops.txt
    -rw-r----- 1 ubuntu ubuntu   87 Feb 23 08:10 notes.txt
    drwxr-xr-x 2 ubuntu ubuntu 4096 Feb 23 08:39 project
    -rwxrw-rw- 1 ubuntu ubuntu   20 Feb 23 08:11 script.sh

3. Document the error messages

when we dont have permission we got this error message:

## -bash: ./script.sh: Permission denied
