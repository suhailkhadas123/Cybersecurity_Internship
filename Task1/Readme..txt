#  Network Port Scanning with Nmap + Wireshark


##  Date: [26-05-2025]

---

##  Objective
Perform a TCP SYN scan on the local network using **Nmap**, and capture the traffic using **Wireshark** to understand how port scanning works at the packet level.

---

##  Tools Used

- **Nmap** – Port scanning
- **Wireshark** – Packet capture & analysis
- **OS:** Windows

---

##  Procedure

### 1. Identified Local IP Range
Using `ipconfig` 
```
IP Range: 192.168.1.0/24
```

### 2. Started Wireshark Capture
- Interface: Active network (Wi-Fi/Ethernet)
- Applied filter after scan:
  ```
  ip.addr == 192.168.1.4 and tcp.flags.syn == 1

  ```

### 3. Ran Nmap SYN Scan
```bash
nmap -sS 192.168.1.0/24
```

### 4. Stopped and Saved Wireshark Capture
- File saved as `nmap_scan_capture.pcapng`

---

##  Scan Results

### 🔹 Host: 192.168.1.1 (Router)

| Port   | State     | Service          | Notes                         |
|--------|-----------|------------------|-------------------------------|
| 21     | Filtered  | FTP              | Possibly firewalled           |
| 22     | Filtered  | SSH              | Remote access, filtered       |
| 23     | Filtered  | Telnet           | Legacy protocol, filtered     |
| 53     | Filtered  | DNS              | Likely used internally        |
| 80     | Open      | HTTP             | Admin interface?              |
| 139    | Filtered  | NetBIOS          | File sharing, filtered        |
| 161    | Filtered  | SNMP             | Network monitoring            |
| 443    | Open      | HTTPS            | Secure web interface          |
| 445    | Filtered  | SMB              | File sharing, firewalled      |
| 3517   | Filtered  | 802.11-IAPP      | Uncommon, IoT/network config  |
| 8082   | Filtered  | BlackICE Alerts  | IDS/IPS, possibly false pos.  |



---

###  Host: 192.168.1.4 (My Laptop)

| Port   | State | Service        | Description                    |
|--------|-------|----------------|--------------------------------|
| 80     | Open  | HTTP           | Local web server               |
| 135    | Open  | MSRPC          | Windows RPC                    |
| 139    | Open  | NetBIOS        | File/printer sharing           |
| 445    | Open  | Microsoft-DS   | SMB for file sharing           |
| 902    | Open  | ISS RealSecure | VMware/Remote control?         |
| 912    | Open  | Apex Mesh      | May be internal software       |
| 5357   | Open  | WSDAPI         | Web services for devices       |

---

##  Risk Analysis

| IP           | Port | Service        | Risk Level | Recommendation                          |
|--------------|------|----------------|------------|------------------------------------------|
| 192.168.1.1  | 80   | HTTP           | High       | Use HTTPS only or block externally       |
| 192.168.1.1  | 443  | HTTPS          | Low        | Keep TLS updated                         |
| 192.168.1.4  | 135  | MSRPC          | Medium     | Firewall internally, block external use  |
| 192.168.1.4  | 139  | NetBIOS        | High       | Disable NetBIOS if not needed            |
| 192.168.1.4  | 445  | SMB            | High       | Disable/patch; vulnerable to ransomware  |
| 192.168.1.4  | 902  | ISS/VM Port    | Medium     | Close if unused                          |
| 192.168.1.4  | 5357 | WSDAPI         | Medium     | Limit device discovery ports             |

---



