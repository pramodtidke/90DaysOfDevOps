# Day 08 – Cloud Server Setup: Docker, Nginx & Web Deployment

## Task: Completed

Today's goal is to **deploy a real web server on the cloud** and learn practical server management.

You will:

- Launch a cloud instance (AWS EC2 or Utho)
- Connect via SSH
- Install Nginx
- Configure security groups for web access (port 80 by default for nginx)
- Extract and save logs to a file
- Verify your webpage is accessible from the internet

I succesfully launch EC2 instance

Connected via SSH and Between two EC2 instance also succesfully.

Command I use:

1. sudo apt-get nginx

2. systemctl status nginx

if nginx is active then i have to take the public ip and in browser search fot http://ip
I must to add port 80 to run web application.

for logs:

3. journalctl -u nginx

to save logs in file

4. journalctl -u nginx > nginx-log.txt

to copy from remote linux server to local machine

5. scp ubuntu@15.135.114.207:~/home/ubuntu/nginx-log.txt /
