# Day 05 – Linux Troubleshooting Drill: CPU, Memory, and Logs

## Task: Completed

Today’s goal is to **run a focused troubleshooting drill**.

You will pick a running process/service on your system and:

- Capture a quick health snapshot (CPU, memory, disk, network)
- Trace logs for that service
- Write a **mini runbook** describing what you did and what you’d do next if things were worse

This turns yesterday’s practice into a repeatable troubleshooting routine.

Linux Troubleshooting Runbook – Day 05

Target service / process : cron

1. uname -a

output:

        ubuntu@ip-172-31-18-13:~$ uname -a
        Linux ip-172-31-18-13 6.14.0-1018-aws #18~24.04.1-Ubuntu SMP Mon Nov 24 19:46:27 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux

note: running ubuntu 24.04 on AWS EC2 x64 bit architecture

2. cat /etc/os-release

output:

          ubuntu@ip-172-31-18-13:~$ cat /etc/os-release
    PRETTY_NAME="Ubuntu 24.04.3 LTS"
    NAME="Ubuntu"
    VERSION_ID="24.04"
    VERSION="24.04.3 LTS (Noble Numbat)"
    VERSION_CODENAME=noble
    ID=ubuntu
    ID_LIKE=debian
    HOME_URL="https://www.ubuntu.com/"
    SUPPORT_URL="https://help.ubuntu.com/"
    BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
    PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
    UBUNTU_CODENAME=noble
    LOGO=ubuntu-logo

note: information regarding Operation system.

3. mkdir /tmp/runbook-demo

output:

note: directory has been created in tmp folder named as a runbook-demo

4. cp /etc/hosts /tmp/runbook-demo/hosts-copy

note: copy from /etc/hosts to /tmp/runbook-demo/hosts-copy
this confirm that disk is writable, has correct permissions, filesystem behave normally.

5. ls -l /tmp/runbook-demo/

output:

ubuntu@ip-172-31-18-13:~$ ls -l /tmp/runbook-demo
total 4
-rw-r--r-- 1 ubuntu ubuntu 221 Feb 20 04:11 hosts-copy

note: this show the permission for the file.

6. top -b -n 1 | head -20

output:

        ubuntu@ip-172-31-18-13:~$ top -b -n 1 | head -20

top - 05:03:51 up 1:12, 2 users, load average: 0.00, 0.00, 0.00
Tasks: 117 total, 1 running, 116 sleeping, 0 stopped, 0 zombie
%Cpu(s): 0.0 us, 4.5 sy, 0.0 ni, 95.5 id, 0.0 wa, 0.0 hi, 0.0 si, 0.0 st
MiB Mem : 914.2 total, 250.3 free, 394.4 used, 427.2 buff/cache  
MiB Swap: 0.0 total, 0.0 free, 0.0 used. 519.8 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
      1 root      20   0   22164  13588   9648 S   0.0   1.5   0:01.52 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_wo+
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      8 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     11 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     13 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     14 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tas+
     15 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tas+
     16 root      20   0       0      0      0 S   0.0   0.0   0:00.03 ksoftir+

note:

✅ System Load
load average: 0.00, 0.00, 0.00

Meaning:
No CPU backlog. System is completely idle.

✔ Excellent health

✅ Process State Summary
Tasks: 117 total
1 running
116 sleeping
0 zombie

Meaning:
Only one active process, rest are sleeping (normal).
No zombie processes → clean system.

✔ Healthy

✅ CPU Usage Breakdown
%Cpu(s): 0.0 us, 4.5 sy, 95.5 id

us (user): user programs → 0%

sy (system): kernel work → 4.5%

id (idle): unused CPU → 95.5%

Meaning:
CPU mostly idle → no heavy workloads.

✔ No CPU bottleneck

✅ Memory Usage
914 MiB total
394 MiB used
250 MiB free
519 MiB available

Meaning:
Over 500 MiB memory available.

✔ No memory pressure
✔ No risk of OOM

✅ Top Processes
PID 1 systemd
0.0% CPU
1.5% MEM

No process consuming high CPU or memory.

✔ No runaway process

🧠 Runbook Note : “System is idle with load average 0.00. CPU is 95% idle and memory has ~520MB available. No high-usage processes observed.”

7. df -h

output:

      ubuntu@ip-172-31-18-13:~$ df -h

Filesystem Size Used Avail Use% Mounted on
/dev/root 19G 2.7G 16G 15% /
tmpfs 458M 0 458M 0% /dev/shm
tmpfs 183M 904K 182M 1% /run
tmpfs 5.0M 0 5.0M 0% /run/lock
efivarfs 128K 3.8K 120K 4% /sys/firmware/efi/efivars
/dev/nvme0n1p16 881M 89M 730M 11% /boot
/dev/nvme0n1p15 105M 6.2M 99M 6% /boot/efi
tmpfs 92M 12K 92M 1% /run/user/1000

notes: df -h shows root filesystem is only 15% used with 16GB free space, and all other partitions have plenty of available space.
Conclusion: No disk space issue; storage is healthy.

8.  free -h
    output:

          ubuntu@ip-172-31-18-13:~$ free -h
                   total        used        free      shared  buff/cache   available

    Mem: 914Mi 394Mi 249Mi 2.7Mi 427Mi 519Mi
    Swap: 0B 0B 0B

note: no memory pressure, we have 520M space is still available

9. ss -tulpn

output:

ubuntu@ip-172-31-18-13:~$ ss -tulpn
Netid State Recv-Q Send-Q Local Address:Port Peer Address:Port Process  
udp UNCONN 0 0 127.0.0.54:53 0.0.0.0:_  
udp UNCONN 0 0 127.0.0.53%lo:53 0.0.0.0:_  
udp UNCONN 0 0 172.31.18.13%ens5:68 0.0.0.0:_  
udp UNCONN 0 0 127.0.0.1:323 0.0.0.0:_  
udp UNCONN 0 0 [::1]:323 [::]:_  
tcp LISTEN 0 4096 127.0.0.54:53 0.0.0.0:_  
tcp LISTEN 0 4096 0.0.0.0:22 0.0.0.0:_  
tcp LISTEN 0 511 0.0.0.0:80 0.0.0.0:_  
tcp LISTEN 0 4096 127.0.0.53%lo:53 0.0.0.0:_  
tcp LISTEN 0 4096 127.0.0.1:39175 0.0.0.0:_  
tcp LISTEN 0 4096 [::]:22 [::]:_  
tcp LISTEN 0 511 [::]:80 [::]:_

note: ss -tulpn shows services listening on port 22 (SSH) and port 80 (HTTP), along with DNS on port 53. No unexpected or suspicious ports are open.
Conclusion: Network services are listening normally and networking layer looks healthy.

10. systemctl status cron

output:

ubuntu@ip-172-31-18-13:~$ systemctl status cron
● cron.service - Regular background program processing daemon
Loaded: loaded (/usr/lib/systemd/system/cron.service; enabled; preset: enabled)
Active: active (running) since Fri 2026-02-20 03:51:41 UTC; 1h 26min ago
Docs: man:cron(8)
Main PID: 535 (cron)
Tasks: 1 (limit: 1017)
Memory: 464.0K (peak: 2.0M)
CPU: 207ms
CGroup: /system.slice/cron.service
└─535 /usr/sbin/cron -f -P

Feb 20 04:55:01 ip-172-31-18-13 CRON[1392]: pam_unix(cron:session): session closed for user root
Feb 20 05:05:01 ip-172-31-18-13 CRON[1529]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 05:05:01 ip-172-31-18-13 CRON[1530]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 05:05:01 ip-172-31-18-13 CRON[1529]: pam_unix(cron:session): session closed for user root
Feb 20 05:15:01 ip-172-31-18-13 CRON[1547]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 05:15:01 ip-172-31-18-13 CRON[1548]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 05:15:01 ip-172-31-18-13 CRON[1547]: pam_unix(cron:session): session closed for user root
Feb 20 05:17:01 ip-172-31-18-13 CRON[1551]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 05:17:01 ip-172-31-18-13 CRON[1552]: (root) CMD (cd / && run-parts --report /etc/cron.hourly)
Feb 20 05:17:01 ip-172-31-18-13 CRON[1551]: pam_unix(cron:session): session closed for user root

note: Cron running ( Active )

11. journalctl - cron -n 50

output :

        ubuntu@ip-172-31-18-13:~$ journalctl -u cron -n 20

Feb 20 04:25:01 ip-172-31-18-13 CRON[1364]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 04:25:01 ip-172-31-18-13 CRON[1363]: pam_unix(cron:session): session closed for user root
Feb 20 04:35:01 ip-172-31-18-13 CRON[1370]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 04:35:01 ip-172-31-18-13 CRON[1371]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 04:35:01 ip-172-31-18-13 CRON[1370]: pam_unix(cron:session): session closed for user root
Feb 20 04:45:01 ip-172-31-18-13 CRON[1381]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 04:45:01 ip-172-31-18-13 CRON[1382]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 04:45:01 ip-172-31-18-13 CRON[1381]: pam_unix(cron:session): session closed for user root
Feb 20 04:55:01 ip-172-31-18-13 CRON[1392]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 04:55:01 ip-172-31-18-13 CRON[1393]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 04:55:01 ip-172-31-18-13 CRON[1392]: pam_unix(cron:session): session closed for user root
Feb 20 05:05:01 ip-172-31-18-13 CRON[1529]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 05:05:01 ip-172-31-18-13 CRON[1530]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 05:05:01 ip-172-31-18-13 CRON[1529]: pam_unix(cron:session): session closed for user root
Feb 20 05:15:01 ip-172-31-18-13 CRON[1547]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 05:15:01 ip-172-31-18-13 CRON[1548]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
Feb 20 05:15:01 ip-172-31-18-13 CRON[1547]: pam_unix(cron:session): session closed for user root
Feb 20 05:17:01 ip-172-31-18-13 CRON[1551]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
Feb 20 05:17:01 ip-172-31-18-13 CRON[1552]: (root) CMD (cd / && run-parts --report /etc/cron.hourly)
Feb 20 05:17:01 ip-172-31-18-13 CRON[1551]: pam_unix(cron:session): session closed for user root

NOTE:
journalctl is a command used to view system logs collected by systemd.

journalctl -u cron -n 20 shows cron is executing scheduled root jobs normally, including system monitoring tasks (debian-sa1) and hourly jobs (/etc/cron.hourly). No errors or failures are present.
Conclusion: cron service is healthy and functioning as expected.

Quick findings:

- cron service status: running
- CPU usage: normal
- Memory usage: normal
- Disk space: healthy
- No resource bottlenecks detected
- Logs show no fatal errors
