

Starting Nmap 7.98 ( https://nmap.org ) at 2025-11-17 14:11 +0530
Nmap scan report for 192.168.1.2
Host is up (0.00043s latency).
Not shown: 993 closed tcp ports (reset)
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
902/tcp  open  iss-realsecure
912/tcp  open  apex-mesh
2179/tcp open  vmrdp
5357/tcp open  wsdapi


Explanation of Open Ports

135/tcp – msrpc (Microsoft RPC)

What it is: Microsoft Remote Procedure Call service.
Used for: Windows services to communicate with each other remotely (DCOM, WMI, etc.).
Security significance:

  * Commonly targeted in Windows attacks (e.g., WannaCry used RPC vulnerabilities).
  * Should not be exposed to the internet.
  * Keep behind a firewall.


139/tcp – netbios-ssn (NetBIOS Session Service)

What it is: Legacy Windows file/printer sharing protocol.
Used for: Older SMB communication before SMB over port 445.
Security significance:

  * Very old protocol, weak security.
  * If enabled, could leak machine names, network info, and allow lateral movement.
  * Safe to disable on modern systems unless needed.


445/tcp – microsoft-ds (SMB — Server Message Block)

What it is: Main Windows file-sharing and domain communication port.
Used for:

  * File sharing
  * Printer sharing
  * Windows authentication
Security significance:

  * One of the most attacked ports (EternalBlue, WannaCry).
  * Must never be exposed publicly.
  * Ensure SMBv1 is disabled.

---

902/tcp – iss-realsecure (VMware Server Console)**

What it is: VMware authentication/remote console service.
Used for:
Communication between VMware host and clients.
Security significance:**

  * Should only be accessible internally.
  * If exposed, could lead to VM access or host compromise.

912/tcp – apex-mesh

What it is: VMware authentication daemon or service related to VMware networking.
Used for:

VMware virtualization components.
Security significance:

  * Check if VMware is installed — expected on hosts with virtualization.
  * Restrict access to local network only.

2179/tcp – vmrdp (VMware Remote Desktop)

What it is: VMware Remote Desktop Protocol (VMRDP).
Used for:

  * Controlling VMs via RDP.
Security significance:

  * Avoid exposing RDP services to external networks.
  * Strong passwords + firewall required.

5357/tcp – wsdapi (Web Services for Devices API)

What it is: Web Services for Devices — used by Windows to discover printers, scanners, and shared devices.
Used for:

  * Device discovery
  * Plug-and-play network devices
Security significance:

  * Usually harmless on home networks.
  * Disable if not using device discovery.

Screenshot of the scan: 

<img width="1347" height="595" alt="cmd_G7rCaXJCRR" src="https://github.com/user-attachments/assets/cd64d661-19a6-4d90-9f03-c18671510e9c" />

