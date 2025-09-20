# **Task 6: DNS Spoofing Attack Simulation**

This repository documents the successful execution of a **DNS Spoofing attack** simulation. The project was completed as part of the BH Cyber Security Internship and demonstrates **Man-in-the-Middle (MITM)** techniques in a safe, isolated environment.**(with screenshots)**

---

### **🎯 Objective**

The primary objective was to successfully perform a **DNS Spoofing attack** in an isolated **VMware** environment using **Kali Linux** as the attacker and **Parrot OS** as the victim. This simulation demonstrates the redirection of a domain name (`example.com`) to a malicious server using a **Man-in-the-Middle (MITM)** technique.

---

### **🛠️ Lab Environment Setup (VMware)**

The lab utilizes a **Host-Only** network configuration to ensure all attack traffic is isolated and contained within the virtual environment.

| Component | OS/Tool | Static IP Address | Role |
| :--- | :--- | :--- | :--- |
| **Attacker** | Kali Linux | `192.168.10.10` | Runs **DNSChef** and the fake web server. |
| **Victim** | Parrot OS | `192.168.10.5` | Initiates the DNS query for `example.com`. |
| **Network** | VMware Workstation | `192.168.10.0/24` subnet | Isolated Host-Only Network (VMnet) with **DHCP Disabled**. |

#### **1. VMware Network Configuration**

* In **VMware Workstation**, the network was configured by selecting or creating a dedicated **Host-Only** network (e.g., VMnet1).
* **Crucially, the DHCP service was disabled** for this network to allow for manual, static IP assignment.

#### **2. VM Network Configuration**

* For both the **Kali** and **Parrot** VMs, the Network Adapter was set to **Custom** and the isolated **Host-Only VMnet** was selected.
* Inside both operating systems, the network interface was manually configured with the static IPs (`192.168.10.10` and `192.168.10.5`).
* **Critical Step (on Parrot OS - Victim):** The **Preferred DNS Server** was set to the **Attacker's IP address** (`192.168.10.10`). This forces all DNS lookups from the victim to go directly to the attacker.
* Communication was verified by running a `ping` test between the two machines.

---

### **😈 Attack Execution Steps**

The attack is launched from the **Kali Linux (Attacker)** machine.

#### **Step 3: Enable IP Forwarding**

This step is essential to allow the attacker to process the victim's traffic while maintaining basic connectivity for the victim.

```bash
# Enable IP forwarding (temporary until reboot)
echo 1 > /proc/sys/net/ipv4/ip_forward
```
#### **Step 4: Setup Fake Web Server (Port 80)**

A simple web page is created and hosted on the **Kali** machine to confirm the successful redirection.

```bash
# 1. Create a confirmation page
echo "<h1>Success! You have been Spoofed! Welcome to the Fake example.com!</h1>" > index.html

# 2. Start Python's simple HTTP server on port 80 (required for the web redirection)
sudo python3 -m http.server 80
```
#### **Step 5: Launch DNSChef (The Spoofing Tool)**

**DNSChef** is the core tool that performs the spoofing by acting as the fake DNS server.

```bash
# -i: Interface IP to listen on (Attacker's IP)
# --spoofdomains: The domain(s) to intercept
# --fakeip: The IP to return (Attacker's IP, hosting the fake page)

dnschef --interface 192.168.10.10 --spoofdomains example.com --fakeip 192.168.10.10
```
#### **Step 6: Trigger the Spoof (Parrot OS)**

Clear the DNS Cache on the **Parrot** machine to force a fresh DNS request:


```bash
sudo systemd-resolve --flush-cache
```
Open the web browser on **Parrot OS** and navigate to:
`http://example.com`

#### **Step 7: Verification**

* **Attacker (Kali - DNSChef):** The terminal log confirms the **interception of the DNS query** for `example.com` and the **injection of the fake IP address**.
* **Victim (Parrot - Browser):** The browser loads the **"Success! You have been Spoofed! Welcome to the Fake example.com!"** page, successfully **confirming the DNS Spoofing and redirection** to the attacker's server.

# Screenshots: 

## Setting and pinging ips in kali(attacker) and parrot(victim) system:

<img width="1919" height="1077" alt="Screenshot 2025-09-20 112635" src="https://github.com/user-attachments/assets/abf74456-924b-4902-9330-924d7a750beb" />
<img width="1917" height="1078" alt="Screenshot 2025-09-20 112600" src="https://github.com/user-attachments/assets/bec8440a-c312-4564-b38e-f97f2513a7f2" />

<img width="1919" height="1079" alt="Screenshot 2025-09-20 112815" src="https://github.com/user-attachments/assets/6e93c530-da08-458c-85a1-1adf906028c2" />
<img width="1917" height="1079" alt="Screenshot 2025-09-20 112925" src="https://github.com/user-attachments/assets/ee233e77-9242-4017-b53e-2e2cbbeb4e35" />


---


## DNS spoofing:


<img width="1918" height="1079" alt="Screenshot 2025-09-20 113633" src="https://github.com/user-attachments/assets/c5f1d562-993c-4423-a23b-ae2e51b6d539" />
<img width="742" height="103" alt="Screenshot 2025-09-20 113414" src="https://github.com/user-attachments/assets/c2df7f8b-b93b-4013-83ab-61b5de4306b1" />

<img width="1919" height="1079" alt="Screenshot 2025-09-20 114302" src="https://github.com/user-attachments/assets/7127d3ba-affd-47d8-acd8-d2b712358ba6" />

<img width="1918" height="1079" alt="Screenshot 2025-09-20 114005" src="https://github.com/user-attachments/assets/c95c28f8-84a9-4aaf-84bb-db80690fe399" />





