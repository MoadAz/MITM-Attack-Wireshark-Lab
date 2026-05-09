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
<br>
<br>
<img width="2562" height="1674" alt="Image" src="https://github.com/user-attachments/assets/3ac9c992-5e9f-4028-82dd-b0043ae70515" />
<br>
<br>
<img width="2715" height="1809" alt="Image" src="https://github.com/user-attachments/assets/d0c2f1cb-0ae7-4099-8027-0bcc166a41e2" />
<br>
<br>
Connectivity between Debian and Fedora:
<br>
<br>
<img width="2460" height="675" alt="Image" src="https://github.com/user-attachments/assets/07666072-4cb6-4fc6-b0e8-7e630da8865b" />

---

## Apache Web Server Setup (Fedora)

Apache server configuration on Fedora :
<br>
<br>
<img width="2583" height="633" alt="Image" src="https://github.com/user-attachments/assets/07f1268b-2471-4db9-8eb0-bb07cd2fbbfe" />
<br>
<br>
---

## Custom Web Page

sudo nano /var/www/html/index.html

Add:

Cybersecurity MITM Lab
<br>
<br>
<img width="2430" height="327" alt="Image" src="https://github.com/user-attachments/assets/28063c65-ab1b-4df3-8bea-8881ba15235c" />
<br>
<br>

---

## HTTP Test

On Debian:

curl http://192.168.244.131

Expected output:

Cybersecurity MITM Lab
<br>
<br>
<img width="2424" height="243" alt="Image" src="https://github.com/user-attachments/assets/a6c4517a-eaba-4f02-9721-28e8247daeb7" />
<br>
<br>

---

## Wireshark Capture

Start Wireshark on Kali:

sudo wireshark

Select interface:
eth0

Start capture.
<br>
<br>
<img width="3390" height="1920" alt="Image" src="https://github.com/user-attachments/assets/6317ffb9-eb1f-4da4-a5e9-04f6f81d0eab" />
<br>
<br>
---

## Generate Traffic

On Debian:

while true; do curl http://192.168.244.131; sleep 2; done
<br>
<br>
<img width="2556" height="567" alt="Image" src="https://github.com/user-attachments/assets/c6fdcaec-2ab1-4e98-85ed-590a08872cb2" />
<br>
<br>


---

## Enable IP Forwarding (Kali)

echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
<br>
<br>
<img width="2628" height="1833" alt="Image" src="https://github.com/user-attachments/assets/787fd8a1-23f8-4b6d-bae1-18e92e91ad5a" />
<br>
<br>

---

## ARP Spoofing Attack

Terminal 1 & 2:

sudo arpspoof -i eth0 -t 192.168.244.128 192.168.244.131
<br>
& 
<br>
sudo arpspoof -i eth0 -t 192.168.244.131 192.168.244.128
<br>
<br>
<img width="3393" height="1911" alt="Image" src="https://github.com/user-attachments/assets/47ca151c-a8f2-4360-82ad-a1baad094872" />
<br>
<br>

---

## Wireshark Analysis

Filter:

http  

You will see:
- HTTP GET requests
- HTTP responses
- Packet interception

Screenshots :
<br>
<br>
<img width="3018" height="1902" alt="Image" src="https://github.com/user-attachments/assets/76f46da3-1e22-474b-8846-a869da672530" />
<br>
---

## Follow HTTP Stream

Right click packet → Follow → HTTP Stream

Example request:

GET / HTTP/1.1  
Host: 192.168.244.131

Response:

Cybersecurity MITM Lab

Screenshot :
<br>
<img width="3390" height="1911" alt="Image" src="https://github.com/user-attachments/assets/d883813d-f408-4243-8934-a923ccc1034b" />

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
