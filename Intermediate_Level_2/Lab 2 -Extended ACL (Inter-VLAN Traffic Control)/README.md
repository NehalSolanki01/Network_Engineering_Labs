Lab 7 – Extended ACL (Inter-VLAN Traffic Control)
🎯 Objective

To implement Extended Access Control Lists (ACLs) to control traffic between VLANs and simulate real-world network security policies.

🧱 Topology
Layer 3 Switch (SVI Routing)
3 VLANs:
VLAN 10 – HR
VLAN 20 – IT
VLAN 30 – Guest

🌐 IP Addressing
VLAN	Network	Gateway
10	192.168.10.0/24	192.168.10.1
20	192.168.20.0/24	192.168.20.1
30	192.168.30.0/24	192.168.30.1
⚙️ Configuration
Enable Routing
ip routing

Configure SVI
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown

interface vlan 20
ip address 192.168.20.1 255.255.255.0
no shutdown

interface vlan 30
ip address 192.168.30.1 255.255.255.0
no shutdown
Extended ACL Configuration
ip access-list extended GUEST-FILTER

deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255

permit tcp 192.168.30.0 0.0.0.255 any eq 80
permit tcp 192.168.30.0 0.0.0.255 any eq 443

deny ip 192.168.30.0 0.0.0.255 any

permit ip any any

Apply ACL
interface vlan 30
ip access-group GUEST-FILTER in

🧪 Verification
Test	Result
Guest → HR	❌ Blocked
Guest → IT	❌ Blocked
Guest → HTTP/HTTPS	✅ Allowed
HR/IT → Guest	✅ Allowed

🧠 Key Learnings
Extended ACLs filter traffic based on multiple parameters
ACLs are processed top-down
Implicit deny exists at the end
Placement of ACL is critical
Layer 3 switches can enforce security policies
