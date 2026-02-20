# Day 07 – Linux File System Hierarchy & Scenario-Based Practice

## Task: Completed

Today's goal is to **understand where things live in Linux** and **practice troubleshooting like a DevOps engineer**.

You will create notes covering:

- Linux File System Hierarchy (the most important directories)
- Practice solving real-world scenarios step by step

This consolidates your Linux fundamentals and prepares you for real-world troubleshooting.

---

Part 1: Linux File System Hierarchy (30 minutes)

- Linux File System Hierarchy (the most important directories)

1. `/` (root) - The starting point of everything
   root of the file system. All directories start here.
   I would use this when navigating root abosolute path.

2) `/home` - User home directories
   It contains the home directories for normal users.
   I would use this when checking usr files

3) `/root` - Root user's home directory
   home directory for root user.
   i would use this when logged in as root user

4) `/etc` - Configuration files
   contains systems and application configuration files
   I would use this when editing the service config

5) `/var/log` - Log files (very important for DevOps!)
   contains the system and service logs
   I would use this when troubleshooting errors.

6) `/tmp` - Temporary files
   Temprory files which is cleared on reboot
   I would use this for testing.

7) `/bin` - Essential command binaries
   I has a system commands. I would this for checking the core commands and binaries

8) `/usr/bin` - User command binaries
   I would check this for installed commands.

9) `/opt` - Optional/third-party applications
   I would use this for custom software installs.

10) '/usr' - user application and binaries
    I would use this for check the user level commands, apps, binaries

11) 'var' - variable data likes logs and caches
    I would use this for cheking system and service logs and caches which is shored in var file sys

12) 'run' - store runtime system information ( Cleared on reeboot)

13) 'sbin' - system binaries(admin level commands)
    I would use this for check the system binaries

## Expected Output

By the end of today, you should have:

- A markdown file named:
  `day-07-linux-fs-and-scenarios.md`

or

- A hand written set of notes (Recommended)

Your notes should have two sections: File System Hierarchy and Scenario Practice.

---

## Guidelines

### Part 1: Linux File System Hierarchy (30 minutes)

- Linux File System Hierarchy (the most important directories)

1. `/` (root) - The starting point of everything
   root of the file system. All directories start here.
   I would use this when navigating root abosolute path.

2) `/home` - User home directories
   It contains the home directories for normal users.
   I would use this when checking usr files

3) `/root` - Root user's home directory
   home directory for root user.
   i would use this when logged in as root user

4) `/etc` - Configuration files
   contains systems and application configuration files
   I would use this when editing the service config

5) `/var/log` - Log files (very important for DevOps!)
   contains the system and service logs
   I would use this when troubleshooting errors.

6) `/tmp` - Temporary files
   Temprory files which is cleared on reboot
   I would use this for testing.

7) `/bin` - Essential command binaries
   I has a system commands. I would this for checking the core commands and binaries

8) `/usr/bin` - User command binaries
   I would check this for installed commands.

9) `/opt` - Optional/third-party applications
   I would use this for custom software installs.

10) '/usr' - user application and binaries
    I would use this for check the user level commands, apps, binaries

11) 'var' - variable data likes logs and caches
    I would use this for cheking system and service logs and caches which is shored in var file sys

12) 'run' - store runtime system information ( Cleared on reeboot)

13) 'sbin' - system binaries(admin level commands)
    I would use this for check the system binaries

**Hands-on task:**

```bash
# Find the largest log file in /var/log
du -sh /var/log/* 2>/dev/null | sort -h | tail -5

ubuntu@ip-172-31-18-13:/$ du -sh /var/log/* 2>/dev/null | sort -h | tail -5
216K	/var/log/sysstat
292K	/var/log/syslog.1
640K	/var/log/syslog
1.1M	/var/log/cloud-init.log
121M	/var/log/journal

# Look at a config file in /etc
cat /etc/hostname

ubuntu@ip-172-31-18-13:/$ cat /etc/hostname
ip-172-31-18-13

# Check your home directory
ls -la ~
``
ubuntu@ip-172-31-18-13:/$ ls -la ~
total 52
drwxr-x--- 6 ubuntu ubuntu 4096 Feb 20 05:57 .
drwxr-xr-x 5 root   root   4096 Jan 31 05:25 ..
-rw------- 1 ubuntu ubuntu 3079 Feb 20 06:44 .bash_history
-rw-r--r-- 1 ubuntu ubuntu  220 Mar 31  2024 .bash_logout
-rw-r--r-- 1 ubuntu ubuntu 3771 Mar 31  2024 .bashrc
drwx------ 2 ubuntu ubuntu 4096 Jan 25 04:48 .cache
drwx------ 4 ubuntu ubuntu 4096 Feb 19 06:33 .config
-rw------- 1 ubuntu ubuntu   20 Feb 20 05:20 .lesshst
-rw-r--r-- 1 ubuntu ubuntu  807 Mar 31  2024 .profile
drwx------ 2 ubuntu ubuntu 4096 Feb 18 03:33 .ssh
-rw-r--r-- 1 ubuntu ubuntu    0 Jan 30 10:13 .sudo_as_admin_successful
-rw------- 1 ubuntu ubuntu 7882 Feb 20 05:57 .viminfo
-rwxrw-r-- 1 ubuntu ubuntu    0 Jan 26 17:07 hello.txt
drwxrwxr-x 2 ubuntu ubuntu 4096 Feb 20 05:57 practice
ubuntu@ip-172-31-18-13:/$


---

### Part 2: Scenario-Based Practice (40 minutes)

**Important:** Focus on understanding the **troubleshooting flow**, not memorizing commands. Use the hints!

---

#### SOLVED EXAMPLE: Understanding How to Approach Scenarios

**Example Scenario: Check if a service is running**

```

Question: How do you check if the 'nginx' service is running?

````

**My Solution (Step by step):**

**Step 1:** Check service status

```bash
systemctl status nginx
````

**Why this command?** It shows if the service is active, failed, or stopped

**Step 2:** If service is not found, list all services

```bash
systemctl list-units --type=service
```

**Why this command?** To see what services exist on the system

**Step 3:** Check if service is enabled on boot

```bash
systemctl is-enabled nginx
```

**Why this command?** To know if it will start automatically after reboot

**What I learned:** Always check status first, then investigate based on what you see.

---

Now try these scenarios yourself:

---

**Scenario 1: Service Not Starting**

```
A web application service called cron failed to start after a server reboot.
What commands would you run to diagnose the issue?

->>>

Step 1: systemctl status cron
Why: I check that if the service running or active or not or it get fails

Step 2: systemctl list-units --type=service
Why:  if the service is not found then check this command

step 3: systemctl is-enabled cron
why: i check the servie is enabled on reboot or not.

step 4: journalctl -u cron -n 50
Why: check logs for unit cron about 50 logs for checks

step 5: systemctl restart cron
Why: we restart the cron to check if it active on boot or not.

here is My Hands on:

        ubuntu@ip-172-31-18-13:~$ systemctl status cron
● cron.service - Regular background program processing daemon
     Loaded: loaded (/usr/lib/systemd/system/cron.service; enabled; preset: enabled)
     Active: active (running) since Fri 2026-02-20 07:47:27 UTC; 1h 18min ago
       Docs: man:cron(8)
   Main PID: 525 (cron)
      Tasks: 1 (limit: 1015)
     Memory: 628.0K (peak: 2.1M)
        CPU: 297ms
     CGroup: /system.slice/cron.service
             └─525 /usr/sbin/cron -f -P

Feb 20 08:35:01 ip-172-31-18-13 CRON[1460]: pam_unix(cron:session): session closed for user root
Feb 20 08:45:01 ip-172-31-18-13 CRON[1468]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 08:45:01 ip-172-31-18-13 CRON[1469]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 08:45:01 ip-172-31-18-13 CRON[1468]: pam_unix(cron:session): session closed for user root
Feb 20 08:55:01 ip-172-31-18-13 CRON[1478]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 08:55:01 ip-172-31-18-13 CRON[1479]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 08:55:01 ip-172-31-18-13 CRON[1478]: pam_unix(cron:session): session closed for user root
Feb 20 09:05:01 ip-172-31-18-13 CRON[1610]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 09:05:01 ip-172-31-18-13 CRON[1611]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 09:05:01 ip-172-31-18-13 CRON[1610]: pam_unix(cron:session): session closed for user root
ubuntu@ip-172-31-18-13:~$ systemctl is-enabled cron
enabled
ubuntu@ip-172-31-18-13:~$ journalctl -u cron -n 20
Feb 20 08:15:01 ip-172-31-18-13 CRON[1429]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 08:15:01 ip-172-31-18-13 CRON[1430]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 08:15:01 ip-172-31-18-13 CRON[1429]: pam_unix(cron:session): session closed for user root
Feb 20 08:17:01 ip-172-31-18-13 CRON[1433]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 08:17:01 ip-172-31-18-13 CRON[1434]: (root) CMD (cd / && run-parts --report /etc/cron.hourly)
Feb 20 08:17:01 ip-172-31-18-13 CRON[1433]: pam_unix(cron:session): session closed for user root
Feb 20 08:25:01 ip-172-31-18-13 CRON[1451]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 08:25:01 ip-172-31-18-13 CRON[1451]: pam_unix(cron:session): session closed for user root
Feb 20 08:35:01 ip-172-31-18-13 CRON[1460]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 08:35:01 ip-172-31-18-13 CRON[1461]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 08:35:01 ip-172-31-18-13 CRON[1460]: pam_unix(cron:session): session closed for user root
Feb 20 08:45:01 ip-172-31-18-13 CRON[1468]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 08:45:01 ip-172-31-18-13 CRON[1469]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 08:45:01 ip-172-31-18-13 CRON[1468]: pam_unix(cron:session): session closed for user root
Feb 20 08:55:01 ip-172-31-18-13 CRON[1478]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 08:55:01 ip-172-31-18-13 CRON[1479]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 08:55:01 ip-172-31-18-13 CRON[1478]: pam_unix(cron:session): session closed for user root
Feb 20 09:05:01 ip-172-31-18-13 CRON[1610]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 09:05:01 ip-172-31-18-13 CRON[1611]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 09:05:01 ip-172-31-18-13 CRON[1610]: pam_unix(cron:session): session closed for user root
ubuntu@ip-172-31-18-13:~$ sudo systemctl restart cron
ubuntu@ip-172-31-18-13:~$ systemctl status cron
● cron.service - Regular background program processing daemon
     Loaded: loaded (/usr/lib/systemd/system/cron.service; enabled; preset: enabled)
     Active: active (running) since Fri 2026-02-20 09:08:52 UTC; 12s ago
       Docs: man:cron(8)
   Main PID: 1628 (cron)
      Tasks: 1 (limit: 1015)
     Memory: 368.0K (peak: 628.0K)
        CPU: 4ms
     CGroup: /system.slice/cron.service
             └─1628 /usr/sbin/cron -f -P

Feb 20 09:08:52 ip-172-31-18-13 systemd[1]: Started cron.service - Regular background program processing daemon.
Feb 20 09:08:52 ip-172-31-18-13 (cron)[1628]: cron.service: Referenced but unset environment variable evaluates to an empty string: EXTRA_OPTS
Feb 20 09:08:52 ip-172-31-18-13 cron[1628]: (CRON) INFO (pidfile fd = 3)
Feb 20 09:08:52 ip-172-31-18-13 cron[1628]: (CRON) INFO (Skipping @reboot jobs -- not system startup)
ubuntu@ip-172-31-18-13:~$



---

**Scenario 2: High CPU Usage**



Your manager reports that the application server is slow.
You SSH into the server. What commands would you run to identify
which process is using high CPU?



step 1: top
Why: As our server is slow, so we have check the which process taking high CPU
so for that we have to use check real time cpu usage.

Step 2: htop
Why: i helps us find the which cpu using high cpu usage.

Step 3: ps aux --sort=-%cpu | head -10
Why : show all running process and short the processes by the CPU usage in descending order( highest first )

step 4 : ps -p 608 -o pid,cmd,%cpu,%mem
Why: “I use ps -p <PID> -o pid,cmd,%cpu,%mem to inspect a specific process and confirm whether it is consuming high CPU or memory.”

ubuntu@ip-172-31-18-13:~$ ps aux --sort=-%cpu | head -10
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         608  0.1  5.0 1802292 47456 ?       Ssl  07:47   0:08 /usr/bin/containerd
root           1  0.0  1.4  22428 13444 ?        Ss   07:47   0:02 /sbin/init
ubuntu      1750  0.0  0.7  15004  7168 ?        S    09:31   0:00 sshd: ubuntu@pts/1
root         778  0.0  7.4 1898348 69524 ?       Ssl  07:47   0:00 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
root        1439  0.0  0.0      0     0 ?        I    08:17   0:00 [kworker/0:2-mm_percpu_wq]
root        1630  0.0  0.0      0     0 ?        I    09:08   0:00 [kworker/1:3-mm_percpu_wq]
ubuntu      1261  0.0  0.7  15004  7240 ?        S    07:47   0:00 sshd: ubuntu@pts/0
root         536  0.0  2.1 1830616 20016 ?       Ssl  07:47   0:00 /snap/amazon-ssm-agent/12322/amazon-ssm-agent
root         181  0.0  2.9 288960 27292 ?        SLsl 07:47   0:00 /sbin/multipathd -d -s

PID - 608 has highest cpu and mem usage.


ubuntu@ip-172-31-18-13:~$ ps -p 608 -o pid,cmd,%cpu,%mem
    PID CMD                         %CPU %MEM
    608 /usr/bin/containerd          0.1  5.0

---

**Scenario 3: Finding Service Logs**



A developer asks: "Where are the logs for the 'docker' service?"
The service is managed by systemd.
What commands would you use?

Step 1:
systemctl status docker
Why: Check service state.

Step 2:
journalctl -u docker -n 50
Why: View recent logs.

Step 3:
journalctl -u docker -f
Why: Follow logs live.

hands on:

      ubuntu@ip-172-31-18-13:~$ systemctl status docker

● docker.service - Docker Application Container Engine
Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: enabled)
Active: active (running) since Fri 2026-02-20 07:47:30 UTC; 2h 8min ago
TriggeredBy: ● docker.socket
Docs: https://docs.docker.com
Main PID: 778 (dockerd)
Tasks: 9
Memory: 89.3M (peak: 90.3M)
CPU: 1.397s
CGroup: /system.slice/docker.service
└─778 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.510963908Z" level=warning msg="Error (Unable to complete atomic operation, key modified) deleting object [endpoint_count b2acc9063509fe5e6ad83b1d05f30f3079aa7df4f68252914245ca8abde0cfea], retrying>
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.565424814Z" level=info msg="Loading containers: done."
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.602940030Z" level=info msg="Docker daemon" commit="28.2.2-0ubuntu1~24.04.1" containerd-snapshotter=false storage-driver=overlay2 version=28.2.2
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.604262411Z" level=info msg="Initializing buildkit"
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.609565371Z" level=warning msg="CDI setup error /etc/cdi: failed to monitor for changes: no such file or directory"
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.609595522Z" level=warning msg="CDI setup error /var/run/cdi: failed to monitor for changes: no such file or directory"
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.633694319Z" level=info msg="Completed buildkit initialization"
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.642887846Z" level=info msg="Daemon has completed initialization"
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.643167396Z" level=info msg="API listen on /run/docker.sock"
Feb 20 07:47:30 ip-172-31-18-13 systemd[1]: Started docker.service - Docker Application Container Engine.

ubuntu@ip-172-31-18-13:~$ journalctl -u docker | head -10
Jan 31 05:34:14 ip-172-31-18-13 systemd[1]: Starting docker.service - Docker Application Container Engine...
Jan 31 05:34:14 ip-172-31-18-13 dockerd[14718]: time="2026-01-31T05:34:14.600846234Z" level=info msg="Starting up"
Jan 31 05:34:14 ip-172-31-18-13 dockerd[14718]: time="2026-01-31T05:34:14.603167932Z" level=info msg="OTEL tracing is not configured, using no-op tracer provider"
Jan 31 05:34:14 ip-172-31-18-13 dockerd[14718]: time="2026-01-31T05:34:14.603591128Z" level=info msg="detected 127.0.0.53 nameserver, assuming systemd-resolved, so using resolv.conf: /run/systemd/resolve/resolv.conf"
Jan 31 05:34:14 ip-172-31-18-13 dockerd[14718]: time="2026-01-31T05:34:14.714012171Z" level=info msg="Creating a containerd client" address=/run/containerd/containerd.sock timeout=1m0s
Jan 31 05:34:14 ip-172-31-18-13 dockerd[14718]: time="2026-01-31T05:34:14.827004070Z" level=info msg="Loading containers: start."
Jan 31 05:34:15 ip-172-31-18-13 dockerd[14718]: time="2026-01-31T05:34:15.156824302Z" level=info msg="Loading containers: done."
Jan 31 05:34:15 ip-172-31-18-13 dockerd[14718]: time="2026-01-31T05:34:15.190319575Z" level=info msg="Docker daemon" commit="28.2.2-0ubuntu1~24.04.1" containerd-snapshotter=false storage-driver=overlay2 version=28.2.2
Jan 31 05:34:15 ip-172-31-18-13 dockerd[14718]: time="2026-01-31T05:34:15.190422457Z" level=info msg="Initializing buildkit"
Jan 31 05:34:15 ip-172-31-18-13 dockerd[14718]: time="2026-01-31T05:34:15.203964167Z" level=warning msg="CDI setup error /etc/cdi: failed to monitor for changes: no such file or directory"

ubuntu@ip-172-31-18-13:~$ journalctl -u docker -f
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.510963908Z" level=warning msg="Error (Unable to complete atomic operation, key modified) deleting object [endpoint_count b2acc9063509fe5e6ad83b1d05f30f3079aa7df4f68252914245ca8abde0cfea], retrying...."
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.565424814Z" level=info msg="Loading containers: done."
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.602940030Z" level=info msg="Docker daemon" commit="28.2.2-0ubuntu1~24.04.1" containerd-snapshotter=false storage-driver=overlay2 version=28.2.2
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.604262411Z" level=info msg="Initializing buildkit"
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.609565371Z" level=warning msg="CDI setup error /etc/cdi: failed to monitor for changes: no such file or directory"
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.609595522Z" level=warning msg="CDI setup error /var/run/cdi: failed to monitor for changes: no such file or directory"
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.633694319Z" level=info msg="Completed buildkit initialization"
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.642887846Z" level=info msg="Daemon has completed initialization"
Feb 20 07:47:30 ip-172-31-18-13 dockerd[778]: time="2026-02-20T07:47:30.643167396Z" level=info msg="API listen on /run/docker.sock"
Feb 20 07:47:30 ip-172-31-18-13 systemd[1]: Started docker.service - Docker Application Container Engine.




---

**Scenario 4: File Permissions Issue**

```

A script at /home/user/backup.sh is not executing.
When you run it: ./backup.sh
You get: "Permission denied"

What commands would you use to fix this?

```


**Step-by-step solution structure:**
Step 1:
ls -l backup.sh
Why: Check permissions.

Step 2:
chmod +x backup.sh
Why: Add execute permission.

Step 3:
ls -l backup.sh
Why: Verify change.

Step 4:
./backup.sh
Why: Run script.
```

```

```
