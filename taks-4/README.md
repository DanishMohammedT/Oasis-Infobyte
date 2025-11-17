
#  **Basic Network Scanning Using Nmap**

This project demonstrates a simple network scan performed using **Nmap** on Windows to identify open ports, running services, and their significance.
It is part of the *Basic Network Scanning* learning task.

---

##  **Objective**

* Install and run Nmap
* Perform a scan on a local machine
* Identify open ports and services
* Understand the purpose and security risks of each open port
* Document findings

---

##  **Tools Used**

* **Nmap** (Windows version)

---

##  **Scan Command Used**

```bash
nmap TARGET
```

#  **Scan Results**

Raw scan output (also stored in `nmap_scan_results.txt`):

```
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
902/tcp  open  iss-realsecure
912/tcp  open  apex-mesh
2179/tcp open  vmrdp
5357/tcp open  wsdapi
```

---

#  **Explanation of Each Open Port**

### **135/tcp — msrpc**

* Microsoft Remote Procedure Call service.
* Used by Windows for remote service communication (WMI, DCOM, etc.).
* **Risk:** Frequently targeted in Windows exploits (e.g., WannaCry). Should not be publicly exposed.

---

### **139/tcp — netbios-ssn**

* Legacy Windows file/printer sharing protocol.
* **Risk:** Outdated protocol — can leak network info. Safe to disable unless required.

---

### **445/tcp — microsoft-ds (SMB)**

* Main Windows file sharing and authentication port.
* **Risk:** One of the most attacked ports (EternalBlue). Never expose to the internet.

---

### **902/tcp — iss-realsecure (VMware)**

* VMware remote console/authentication port.
* **Risk:** Should be restricted to internal networks to avoid VM compromise.

---

### **912/tcp — apex-mesh (VMware)**

* VMware networking/auth daemon.
* **Risk:** Normal on virtualized systems. Restrict access to LAN only.

---

### **2179/tcp — vmrdp (VMware Remote Desktop)**

* Used for RDP access to VMware virtual machines.
* **Risk:** Requires strong passwords + firewall. Never expose externally.

---

### **5357/tcp — wsdapi**

* Web Services for Devices API.
* Used for device discovery (printers, scanners).
* **Risk:** Low. Safe on local networks; can be disabled if unnecessary.

---

#  **Project Deliverables**

| File                      | Description                              |
| ------------------------- | ---------------------------------------- |
| **nmap_scan_results.txt** | Contains the raw Nmap output.            |
| **README.md**             | Explanation of scan results (this file). |
| **Screenshots**           | Screenshots of Nmap command and output.  |

---

#  **Conclusion**

This activity provided hands-on experience with:

* Running Nmap on Windows
* Understanding port scanning
* Identifying services running on a system
* Evaluating potential security risks

This is the foundation for further network security analysis.

