# Network Topology Verification - Corporate VPN with Centralized Services


This document confirms the successful configuration and validation of the corporate network lab, featuring a Head Office (HO), two Branch Offices (BR1, BR2), 
centralized DHCP/File/Print services, and a secure WAN using GRE tunnels and EIGRP.

---

## 1. Design & Core Services

| Feature                      | Verification Method                                                    | Status    |
| :--------------------------- | :--------------------------------------------------------------------- | :-------- |
| **Centralized DHCP**         | BR1/BR2 PCs received IPs from 192.168.100.10 via `ip helper-address`.  | ✅ Passed |
| **Inter-VLAN Routing (ROAS)**| Pings between HO VLANs (10/20/30) and Server VLAN (100) successful.    | ✅ Passed |
| **WAN Tunnel Status**        | `show interface tunnel 1/2` confirmed line protocol is UP.             | ✅ Passed |
| **Dynamic Routing**          | `Route_HO# show ip route eigrp` displayed remote BR1/BR2 networks.     | ✅ Passed |

## 2. Critical Connectivity Validation

The following tests confirm end-to-end communication across the entire topology.

| Test ID | Test Goal            | Source Device | Destination Device | Command       | Result             | Screenshot              |
| :---    | :------------------- | :------------ | :--------- | :-------------------- | :----------------- | :---------------------- |
| **W1**  | BR1 Access HO Server | PC_BR1        | FileSVR    | `ping 192.168.100.11` | Success            | `W1_BR1_to_FileSVR.png` |
| **W3**  | Branch-to-Branch     | PC_BR1        | PC_BR2     | `ping 192.168.60.100` | Success            | `W3_BR1_to_BR2.png`     |
| **L3**  | HO Wireless IP       | IT Laptop     | N/A        | `ipconfig`            | IP in 192.168.30.x | `L3_IT_Wireless_IP.png` |

## 3. Security Validation (ACL Enforcement)

The Extended ACL requirement to block Finance traffic from accessing the File Server was successfully verified on Route_HO.

| Test ID         | Test Goal           | Source Device | Destination Device | Command               | Result      | Screenshot             |
| :-------------- | :------------------ | :------------ | :----------------- | :-------------------- | :---------- | :--------------------- |
| **S1 (Deny)**   | Finance Restriction | PC_FN1        | FileSVR            | `ping 192.168.100.11` | **Failure** | `S1_FN_ACL_Deny.png`   |
| **S2 (Permit)** | HR Access           | PC_HR1        | FileSVR            | `ping 192.168.100.11` | Success     | `S2_HR_ACL_Permit.png` 

---
