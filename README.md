# 🌐 Enterprise Network Redundancy
### HSRP | EIGRP | SSH | HTTP Server
> Cisco Packet Tracer Project

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue)
![CCNA](https://img.shields.io/badge/Level-CCNA-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

## 📋 Project Overview
A fully redundant enterprise network simulation demonstrating
real-world high availability using dual routers, dynamic routing,
secure remote access, and HTTP server reachability.

---

## 🗺️ Topology
- ISP Router (top) → HTTP Server (10.1.1.2)
- R-1 uplink: 2.1.1.0/24
- R-2 uplink: 1.1.1.0/24
- Both R-1 and R-2 → Switch0 → 8 PCs
- Virtual Gateway: 192.168.1.1

---

## ⚙️ Technologies Used
| Technology | Purpose |
|---|---|
| HSRP | Gateway redundancy |
| EIGRP AS 100 | Dynamic routing |
| SSH | Secure remote access |
| HTTP Server | Web reachability test |
| Cisco IOS | All device configuration |

---

## 🔧 Device Configuration

### R-1 (Standby — Priority 100)
​```
interface GigabitEthernet0/1
 ip address 192.168.1.2 255.255.255.0
 standby 1 ip 192.168.1.1
 standby 1 priority 100
 standby 1 preempt
 no shutdown

interface GigabitEthernet0/0
 ip address 2.1.1.1 255.255.255.0
 no shutdown

router eigrp 100
 network 192.168.1.0 0.0.0.255
 network 2.1.1.0 0.0.0.255
​```

### R-2 (Active — Priority 110)
​```
interface GigabitEthernet0/1
 ip address 192.168.1.3 255.255.255.0
 standby 1 ip 192.168.1.1
 standby 1 priority 110
 standby 1 preempt
 no shutdown

interface GigabitEthernet0/2
 ip address 1.1.1.1 255.255.255.0
 no shutdown

router eigrp 100
 network 192.168.1.0 0.0.0.255
 network 1.1.1.0 0.0.0.255

enable secret maaz
ip domain-name khan.com
crypto key generate rsa
username user1 privilege 15 secret user1
line vty 0 1
 login local
 transport input telnet
​```

### IPS Router (SSH Server)
​```
hostname IPS
interface GigabitEthernet0/2
 ip address 1.1.1.2 255.255.255.0
interface GigabitEthernet0/0
 ip address 2.1.1.2 255.255.255.0
interface GigabitEthernet0/1
 ip address 10.1.1.1 255.255.255.0

router eigrp 100
 network 10.1.1.0 0.0.0.255
 network 1.1.1.0 0.0.0.255
 network 2.1.1.0 0.0.0.255

enable secret maaz
ip domain-name khan.com
crypto key generate rsa
username user1 privilege 15 secret user1
username user2 privilege 15 secret user2
line vty 0 4
 login local
 transport input ssh
​
 ✅ Test Results

| Test | Command | Result |
|---|---|---|
| HSRP Status | show standby brief | ✅ Active/Standby verified |
| Failover Test | shutdown Gig0/1 on R-2 | ✅ R-1 became Active |
| Preempt Test | no shutdown on R-2 | ✅ R-2 reclaimed Active |
| EIGRP Neighbors | show ip eigrp neighbors | ✅ Dual adjacency |
| SSH Access | ssh -l user1 1.1.1.2 | ✅ Secure login |
| HTTP Access | Browser → 10.1.1.2 | ✅ Page loaded |
| PC Ping | ping 192.168.1.1 | ✅ 0% loss |

---

 📊 HSRP States Observed
Normal:   R-2 = Active  | R-1 = Standby
Failure:  R-2 = Down    | R-1 = Active
Recovery: R-2 = Active  | R-1 = Standby (preempt)

---

## 📁 Files
- `HSRP.pkt` — Cisco Packet Tracer file
- `README.md` — This documentation

---

## 👤 Author
**Maaz Khan**
Network Enigineer | CCNA | 110+ Projects |
[LinkedIn](https://www.linkedin.com/in/maazkhanms/)
[GitHub](https://github.com/Maazkhan7)

 
