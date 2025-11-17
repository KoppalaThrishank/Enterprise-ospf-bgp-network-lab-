# Enterprise-ospf-bgp-network-lab-

In this lab, we started with the basic concept that we usually learn in the beginning of networking:
keeping all PCs inside a single broadcast domain (VLAN 1) makes communication simple.
In that scenario, all devices stay in the same subnet, and a Layer-3 router can forward traffic without requiring multiple VLANs.
At this level, VLANs are not necessary — everything operates in one flat network.

However, real-world enterprise networks never rely on a single VLAN.
Organizations separate departments, servers, and security zones into different VLANs, each mapped to its own IP subnet.
Because of this separation, devices in different VLANs cannot communicate without routing.

To support this, enterprises use Inter-VLAN Routing on a Layer-3 switch, allowing traffic to move between different VLANs/subnets.
At the same time, security is enforced:

Firewalls can allow or block traffic between VLANs

ACLs can restrict communication based on IP, protocol, or port

VLANs act as logical security boundaries inside the network






# Enterprise Multi-Area OSPF + eBGP + Layer-3 Switching Lab  
A full end-to-end enterprise network topology built on **EVE-NG**, demonstrating:

✔ Multi-Area OSPF (Area 0, Area 1, Area 2)  
✔ ASBR connecting OSPF → BGP domains  
✔ External BGP peering with simulated ISP  
✔ Layer-3 Core Switch with SVIs for VLAN routing  
✔ Routed Port uplinks (no switchport)  
✔ L2/L3 separation for access and core  
✔ VLANs (10, 100) with static IP end devices  
✔ Full troubleshooting of return path and OSPF advertisement issues  

 all end devices were configured with static IPs.

---

## 📌 1. Topology Overview
The lab contains:

- **Access Switch 2 & 3 (Layer-2 only)**  
- **Core Layer-3 Switch**  
  - Performs Inter-VLAN routing  
  - Runs OSPF with routers  
- **OSPF Routers in Area 0, Area 1, Area 2**  
- **ASBR Router** (OSPF ↔ BGP redistribution)  
- **eBGP Router** peering with simulated ISP  
- **Edge Router** providing default route to OSPF domain  
- **VLANs**: 10,20,30,40,50,60,70,80,90,100  
- **PCs with static IPs**  

.

---

## 📌 2. VLAN & IP Addressing

| VLAN | Subnet           | Gateway (SVI on Core Switch) |
| ---- | ---------------- | ---------------------------- |
| 10   | 192.168.1.1/24  | 192.168.1.10                 |
| 20   | 192.168.2.1/24  | 192.168.2.20|
| 30   | 192.168.3.1/24  | 192.168.3.30                 |
| 40   | 192.168.4.1/24  | 192.168.4.40                 |
| 50   | 192.168.5.1/24  | 192.168.5.50                 |
| 60   | 192.168.6.1/24  | 192.168.6.60                 |
| 70   | 192.168.7.1/24  | 192.168.7.70                 |
| 80   | 192.168.8.1/24  | 192.168.8.80                 |
| 90   | 192.168.9.1/24  | 192.168.9.90                 |
| 100  | 192.168.10.1/24 | 192.168.10.100                |


## 📌 3. Layer-3 Core Switch
The core switch performs:

- Inter-VLAN routing using SVIs  
- Routed uplink to Area 1 Router  
- OSPF adjacency on routed port  
- Advertises VLAN 10 and VLAN 100 into OSPF  

.

---

## 📌 4. OSPF Design
### Areas:
- **Area 0** → Backbone router  
- **Area 1** → Core Switch ↔ Area 1 router link  
- **Area 2** → ASBR connecting to eBGP router  

VLAN SVIs are advertised inside their AREA (configured in CoreSwitch).

The physical routed link **must be advertised in OSPF**  
(otherwise VLAN PCs cannot ping eBGP physical IP).

---

## 📌 5. BGP Design
- eBGP router peers with ISP  
- ASBR redistributes BGP → OSPF  
- Loopback used for BGP ID  
- Only OSPF-side links advertised  
- WAN interface is NOT reachable from VLANs (correct design)

.


✔ What You'll Observe in This Lab

All VLANs (10 and 100) can successfully communicate with each other through Inter-VLAN Routing on the Core Layer-3 Switch.

All VLANs are also able to reach the ISP through internal OSPF routing and external eBGP connectivity.

This demonstrates a real enterprise routing flow:
---

## 📌 6. Key Learning Outcomes
- Why loopback was pinging but physical IP failed (missing OSPF network command)  
- Why VLAN PCs use **CoreSwitch SVI** as gateway, not router  
- Why `no switchport` is required for routed uplinks  
- How return paths and OSPF advertisement affect reachability  
- Why routers can ping each other but PCs cannot  
- eBGP WAN interface can’t be pinged from inside LAN (correct behavior)




