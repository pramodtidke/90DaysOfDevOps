# Day 04 – Linux Practice: Processes and Services

## Task: Completed

1.  Check running processes
    1.  command:

        ps aux

        output:

                        USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND

              root 1 0.0 1.4 22392 13596 ? Ss 06:31 0:01 /sbin/init
              root 2 0.0 0.0 0 0 ? S 06:31 0:00 [kthreadd]
              root 3 0.0 0.0 0 0 ? S 06:31 0:00 [pool_workqueue_release]
              root 4 0.0 0.0 0 0 ? I< 06:31 0:00 [kworker/R-rcu_gp]
              root 5 0.0 0.0 0 0 ? I< 06:31 0:00 [kworker/R-sync_wq]
              root 6 0.0 0.0 0 0 ? I< 06:31 0:00 [kworker/R-kvfree_rcu_reclaim]
              root 7 0.0 0.0 0 0 ? I< 06:31 0:00 [kworker/R-slub_flushwq]
              root 8 0.0 0.0 0 0 ? I< 06:31 0:00 [kworker/R-netns]
              root 11 0.0 0.0 0 0 ? I< 06:31 0:00 [kworker/0:0H-events_highpri]
              root 13 0.0 0.0 0 0 ? I< 06:31 0:00 [kworker/R-mm_percpu

        what is saw and learned:

        Each running process has its own PID
        i can indentify the heavy process by %CPU & %MEM
        PID 1 is a systemd

    2.  Command: to quickly gives the PID of the process by name

        pgrep

        output:

              1143

        what is saw and learned:

            pgrep quickly gives the PID of a process by name

2.  Inspect one systemd service

    command:

          systemctl status nginx

    output:

                  ubuntu@ip-172-31-18-13:~$ systemctl status nginx
        ● nginx.service - A high performance web server and a reverse proxy server
            Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
            Active: active (running) since Thu 2026-02-19 06:31:26 UTC; 3h 46min ago
              Docs: man:nginx(8)
            Process: 544 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
            Process: 562 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
          Main PID: 583 (nginx)
              Tasks: 3 (limit: 1017)
            Memory: 3.6M (peak: 3.9M)
                CPU: 29ms
            CGroup: /system.slice/nginx.service
                    ├─583 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
                    ├─591 "nginx: worker process"
                    └─593 "nginx: worker process"

        Feb 19 06:31:25 ip-172-31-18-13 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
        Feb 19 06:31:26 ip-172-31-18-13 systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.

    what is saw and learned:

          Service is running

          Shows main PID and service state

3.  Capture a small troubleshooting flow

    Website is not working ?

    step 1: checks internet

            ping google.com

    output:

                    ubuntu@ip-172-31-18-13:~$ ping google.com
            PING google.com (142.250.183.46) 56(84) bytes of data.
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=1 ttl=119 time=1.08 ms
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=2 ttl=119 time=1.11 ms
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=3 ttl=119 time=1.11 ms
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=4 ttl=119 time=1.14 ms
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=5 ttl=119 time=1.10 ms
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=6 ttl=119 time=1.14 ms
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=7 ttl=119 time=1.12 ms
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=8 ttl=119 time=1.11 ms
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=9 ttl=119 time=1.11 ms
            q64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=10 ttl=119 time=1.13 ms
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=11 ttl=119 time=1.11 ms
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=12 ttl=119 time=1.11 ms
            64 bytes from pnsyda-aj-in-f14.1e100.net (142.250.183.46): icmp_seq=13 ttl=119 time=1.12 ms
            ^C
            --- google.com ping statistics ---
            13 packets transmitted, 13 received, 0% packet loss, time 12015ms
            rtt min/avg/max/mdev = 1.080/1.114/1.143/0.015 ms

    step 2: checks ip address

            ip addr

    output:

                    ubuntu@ip-172-31-18-13:~$ ip addr

            1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
            link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
            inet 127.0.0.1/8 scope host lo
            valid_lft forever preferred_lft forever
            inet6 ::1/128 scope host noprefixroute
            valid_lft forever preferred_lft forever
            2: ens5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc mq state UP group default qlen 1000
            link/ether 0a:fc:6e:0b:9b:d7 brd ff:ff:ff:ff:ff:ff
            inet 172.31.18.13/20 metric 100 brd 172.31.31.255 scope global dynamic ens5
            valid_lft 2274sec preferred_lft 2274sec
            inet6 fe80::8fc:6eff:fe0b:9bd7/64 scope link
            valid_lft forever preferred_lft forever
            3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
            link/ether fe:3a:3d:0e:94:45 brd ff:ff:ff:ff:ff:ff
            inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
            valid_lft forever preferred_lft forever

    step 3: check DNS

            dig google.com

    output:

            ubuntu@ip-172-31-18-13:~$ dig google.com

        ; <<>> DiG 9.18.39-0ubuntu0.24.04.2-Ubuntu <<>> google.com
        ;; global options: +cmd
        ;; Got answer:
        ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 32332
        ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

        ;; OPT PSEUDOSECTION:
        ; EDNS: version: 0, flags:; udp: 65494
        ;; QUESTION SECTION:
        ;google.com.			IN	A

        ;; ANSWER SECTION:
        google.com.		172	IN	A	142.250.195.238

        ;; Query time: 3 msec
        ;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
        ;; WHEN: Thu Feb 19 10:23:45 UTC 2026
        ;; MSG SIZE  rcvd: 55

    step 4: test https layer

    curl google.com

    output:

                ubuntu@ip-172-31-18-13:~$ curl google.com
        <HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
        <TITLE>301 Moved</TITLE></HEAD><BODY>
        <H1>301 Moved</H1>
        The document has moved
        <A HREF="http://www.google.com/">here</A>.
        </BODY></HTML>

    what i did and learned:

          I troubleshoot networking using a layered approach: first ping to test connectivity, then ip addr to verify IP configuration,
          dig to check DNS resolution, and finally curl to validate HTTP response. If all pass, the network stack is healthy.
