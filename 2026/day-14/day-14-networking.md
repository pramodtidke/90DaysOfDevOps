# Day 14 – Networking Fundamentals & Hands-on Checks

## Task

Get comfortable with core networking concepts and the commands you’ll actually run during troubleshooting.

You will:

- Map the **OSI vs TCP/IP models** in your own words
- Run essential connectivity commands
- Capture a mini network check for a target host/service

Keep it short, real, and repeatable.

---

---

## Quick Concepts (write 1–2 bullets each)

- OSI layers (L1–L7) vs TCP/IP stack (Link, Internet, Transport, Application)
- Where **IP**, **TCP/UDP**, **HTTP/HTTPS**, **DNS** sit in the stack
- One real example: “`curl https://example.com` = App layer over TCP over IP”

---

## Hands-on Checklist (run these; add 1–2 line observations)

- **Identity:** `hostname -I` (or `ip addr show`) — note your IP.
  ubuntu@ip-172-31-2-91:~$ hostname -I
  172.20.2.91 172.17.0.1 172.18.0.1
  --> hostname with address

- **Reachability:** `ping <target>` — mention latency and packet loss.
  ubuntu@ip-172-31-2-91:~$ ping trainwithshubham.com
  PING trainwithshubham.com (3.33.251.168) 56(84) bytes of data.
  64 bytes from aec037177372cc6cd.awsglobalaccelerator.com (3.33.251.168): icmp_seq=1 ttl=250 time=0.687 ms
  64 bytes from aec037177372cc6cd.awsglobalaccelerator.com (3.33.251.168): icmp_seq=2 ttl=250 time=0.684 ms
  64 bytes from aec037177372cc6cd.awsglobalaccelerator.com (3.33.251.168): icmp_seq=3 ttl=250 time=0.681 ms
  64 bytes from aec037177372cc6cd.awsglobalaccelerator.com (3.33.251.168): icmp_seq=4 ttl=250 time=0.696 ms
  64 bytes from aec037177372cc6cd.awsglobalaccelerator.com (3.33.251.168): icmp_seq=5 ttl=250 time=0.691 ms
  64 bytes from aec037177372cc6cd.awsglobalaccelerator.com (3.33.251.168): icmp_seq=6 ttl=250 time=0.696 ms
  64 bytes from aec037177372cc6cd.awsglobalaccelerator.com (3.33.251.168): icmp_seq=7 ttl=250 time=0.757 ms
  ^C
  --- trainwithshubham.com ping statistics ---
  7 packets transmitted, 7 received, 0% packet loss, time 6105ms
  rtt min/avg/max/mdev = 0.681/0.698/0.757/0.024 ms

Average latency: 0.698 ms
That is extremely fast.

no packates loss as system send 7 ICMP ping request and server reply with all 7.

- **Path:** `traceroute <target>` (or `tracepath`) — note any long hops/timeouts.
  ubuntu@ip-172-31-2-91:~$ traceroute trainwithshubham.com
  traceroute to trainwithshubham.com (3.33.251.168), 30 hops max, 60 byte packets
  1 244.5.0.127 (244.5.0.127) 2.891 ms _ 244.5.0.129 (244.5.0.129) 4.332 ms
  2 240.1.184.4 (240.1.184.4) 0.237 ms 0.208 ms 240.1.184.7 (240.1.184.7) 0.226 ms
  3 240.2.216.9 (240.2.216.9) 1.234 ms 240.2.216.10 (240.2.216.10) 1.218 ms 240.2.216.11 (240.2.216.11) 1.230 ms
  4 54.240.203.123 (54.240.203.123) 2.410 ms 2.374 ms 54.240.203.131 (54.240.203.131) 2.372 ms
  5 _ \* _
  6 _ \* _
  7 _ \* \*

  -->traceroute sends packets with increasing TTL (Time To Live) values.
  maximum 30 hops router traceroute will checks.

- **Ports:** `ss -tulpn` (or `netstat -tulpn`) — list one listening service and its port.
  ubuntu@ip-172-31-2-91:~$ ss -tulpn
  Netid State Recv-Q Send-Q Local Address:Port Peer Address:Port Process  
  udp UNCONN 0 0 127.0.0.54:53 0.0.0.0:_  
  udp UNCONN 0 0 127.0.0.53%lo:53 0.0.0.0:_  
  udp UNCONN 0 0 172.31.2.91%ens5:68 0.0.0.0:_  
  udp UNCONN 0 0 127.0.0.1:323 0.0.0.0:_  
  udp UNCONN 0 0 [::1]:323 [::]:_  
  tcp LISTEN 0 511 0.0.0.0:80 0.0.0.0:_  
  tcp LISTEN 0 4096 0.0.0.0:22 0.0.0.0:_  
  tcp LISTEN 0 4096 127.0.0.54:53 0.0.0.0:_  
  tcp LISTEN 0 4096 127.0.0.53%lo:53 0.0.0.0:_  
  tcp LISTEN 0 4096 127.0.0.1:41649 0.0.0.0:_  
  tcp LISTEN 0 511 [::]:80 [::]:_  
  tcp LISTEN 0 4096 [::]:22 [::]:_

- **Name resolution:** `dig <domain>` or `nslookup <domain>` — record the resolved IP.
  ubuntu@ip-172-31-2-91:~$ dig trainwithshubham.com

; <<>> DiG 9.18.39-0ubuntu0.24.04.2-Ubuntu <<>> trainwithshubham.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 31586
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;trainwithshubham.com. IN A

;; ANSWER SECTION:
trainwithshubham.com. 300 IN A 3.33.251.168
trainwithshubham.com. 300 IN A 15.197.225.128

;; Query time: 2 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Wed Mar 11 09:29:43 UTC 2026
;; MSG SIZE rcvd: 81

ubuntu@ip-172-31-2-91:~$ nslookup trainwithshubham.com
Server: 127.0.0.53
Address: 127.0.0.53#53

Non-authoritative answer:
Name: trainwithshubham.com
Address: 15.197.225.128
Name: trainwithshubham.com
Address: 3.33.251.168

- **HTTP check:** `curl -I <http/https-url>` — note the HTTP status code.

- **Connections snapshot:** `netstat -an | head` — count ESTABLISHED vs LISTEN (rough).

ubuntu@ip-172-31-2-91:~$ netstat -an | head
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address Foreign Address State  
tcp 0 0 0.0.0.0:80 0.0.0.0:_ LISTEN  
tcp 0 0 0.0.0.0:22 0.0.0.0:_ LISTEN  
tcp 0 0 127.0.0.54:53 0.0.0.0:_ LISTEN  
tcp 0 0 127.0.0.53:53 0.0.0.0:_ LISTEN  
tcp 0 0 127.0.0.1:41649 0.0.0.0:_ LISTEN  
tcp 0 336 172.31.2.91:22 223.185.42.94:21788 ESTABLISHED
tcp6 0 0 :::80 :::_ LISTEN  
tcp6 0 0 :::22 :::\* LISTEN

Pick one target service/host (e.g., `google.com`, your lab server, or a local service) and stick to it for ping/traceroute/curl where possible.

---

## Mini Task: Port Probe & Interpret

1. Identify one listening port from `ss -tulpn` (e.g., SSH on 22 or a local web app).
2. From the same machine, test it: `nc -zv localhost <port>` (or `curl -I http://localhost:<port>`).
3. Write one line: is it reachable? If not, what’s the next check? (e.g., service status, firewall).

---

## Reflection (add to your markdown)

- Which command gives you the fastest signal when something is broken?
  --> ping : Why ping?

It quickly checks:

Network connectivity, Latency, Packet loss, Host availability

- What layer (OSI/TCP-IP) would you inspect next if DNS fails? If HTTP 500 shows up?
  DNS works at: Application Layer (OSI Layer 7)

  DNS operates at the Application Layer (Layer 7), so I would first check the DNS resolver using tools like dig or nslookup. If needed, I would also verify connectivity to port 53 over UDP/TCP.

- Two follow-up checks you’d run in a real incident.
  In real DevOps incidents, after basic checks, I usually run:

1️⃣ Service health check
systemctl status nginx

or

docker ps

Checks if the service is running.

2️⃣ Port listening check
ss -tulnp

or

netstat -tulnp

Example output:

tcp LISTEN 0 128 0.0.0.0:80

Confirms service is listening.

Other useful checks
curl localhost:80
top
df -h

---
