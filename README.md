# Integrated Enterprise Network Lab (COS370 & COS372)

Hello, I'm Abdulkarim Alshehri, a Computer Science (Networks) student in KSU.
tools i used in this lab (GNS3, Oracle VirtualBox, Cisco IOS, Windows Server 2012, Linux CentOS)

---

## What is this?
This is my final conclusion project for my Network Protocols and Switching/Routing courses. The challenge was to build a fully functional "mini-internet" inside a single laptop.

I had to simulate a real company network with 4 branches, all connected together, where employees can send emails, transfer files, make VoIP calls, and browse an internal website.

## The Challenge (The "Potato Laptop" Problem)
The hardest part wasn't the routing it was the hardware.
The project required **9 Virtual Machines** (Servers + Clients) and **4 Cisco Routers** running at the same time.
* **Constraint:** My laptop only has 16GB RAM.
* **Solution:** I used **Linked Clones** in VirtualBox and optimized the RAM for each server down to less than 1.5GB. This allowed me to run the entire data center on my personal machine without crashing.

## How I Built It
### 1. The Core Network (Cisco)
I used 4 Cisco routers connected in a **Full Mesh** topology (everything connects to everything).
* **Routing Protocol:** OSPF (Area 0).
* **Why:** If one cable gets cut, the network automatically reroutes traffic so no one loses connection.

### 2. The Servers (Windows & Linux)
I set up a dedicated server for each protocol:
* **Web Server (IIS):** Hosts the company intranet site.
* **DNS Server:** Translates IPs so users can type `myweb.website.local` instead of numbers.
* **FTP Server:** A secure file storage where users have to log in to download files.
* **SMTP Server:** Handles email between users.
* **VoIP Server (Linux):** I replaced a Windows server with a lightweight CentOS Linux VM running Asterisk/FreePBX to handle phone calls.

## The Hardest Bugs I Fixed
This wasn't just "plug and play." Here are the two biggest headaches I solved:

**1. The FTP "Firewall" Trap**
I could log in to the FTP server, but I couldn't see any files. It turned out that the FTP "Data Channel" uses random ports that the Windows Firewall was blocking.
* *Fix:* I diagnosed the Passive Mode issue and reconfigured the Windows Firewall rules to allow the traffic.

**2. The Linux "Root" Lockout**
For the VoIP server, the web dashboard wouldn't load. I realized the Linux firewall (`iptables`) was blocking Port 80. I had to hack into the server using Single User Mode to reset the root password and shut down the firewall from the command line.

---

### Screenshots
*(See the /screenshots folder for more)*

**The Network Topology:**
![Topology](Network_Topology.png)
