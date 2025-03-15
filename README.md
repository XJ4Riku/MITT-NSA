# Network Proposal for Open House Gaming Exhibition

![techbridge](https://github.com/user-attachments/assets/53d42306-0aab-4c0f-b254-eb2a42495f9a) 

### Executive Summary
- The Open House Exhibition 2024 aims to bring together gaming enthusiasts, developers, and industry professionals in an interactive environment showcasing the latest in gaming technology, esports, and innovation. This event will foster networking opportunities, provide a platform for new game launches, and create unforgettable experiences for attendees.

### Objectives  
- Highlight emerging trends in gaming technology and innovation.
- Create a space for esports competitions, workshops, and interactive demos.
- Attract gaming enthusiasts and professionals for knowledge exchange and networking.
- Design a robust and scalable network infrastructure to support the gaming exhibition throughout the event.  
- Ensure high availability, reliable connectivity, and future scalability.  
- Implement network segmentation for security and efficiency.
 ### Proposed Activities
- **Exhibition Zones:**
  - Game Demos: Showcase the latest and upcoming games from leading and indie developers.
  - Tech Showcase: Display cutting-edge gaming hardware and software, including VR and AR.
  - Industry Talks: Keynote presentations from gaming industry leaders and innovators.
- **Esports Arena**
  - Host competitive tournaments for popular games (e.g., Minecraft, League of Legends, Fortnite, Valorant).
  - Live-stream the events to online audiences.
- **Workshops and Panels**
  - Game Development Workshops: Hands-on sessions for budding developers.
  - Panels: Discussions on topics like game design, storytelling, and industry trends.
- **Interactive Zones**
  - VR and AR gaming experiences.
  - Retro gaming zone for nostalgia and fun.

### Networking
---
### Network Design and Topology  
- **Physical Topology:**

![Picture4 topo ](https://github.com/user-attachments/assets/c454ac5d-1372-4980-8d4e-133778e32ead)

- **Logical Topology:**
  
- ![Picture3 topo](https://github.com/user-attachments/assets/7e9c9a1e-ef95-4d75-8fbd-976ce9b3ca7e)
  - The above topology uses a hybrid topology combining star and hierarchical models for optimized connectivity.
  - **Zonal Segmentation**: The network is divided into zones, such as the "Techbridge zone which is the primary company" and "Open House Event Zone," each with its own devices and purpose. This reflects elements of segmentation for organization and performance.
  - **Integration of Wired and Wireless Networks**: Wired connections (e.g., Cat 5e cables) are used for PCs, servers, and switches, while wireless access points enable BYOD (Bring Your Own Device) connectivity. which is a hallmark of hybrid setups.
    
- **IP Addressing Scheme:**  
  - Subnet allocation for departments (e.g., IT: 192.168.20.0/24, Sales: 192.168.40.0/24).  
  - Static IPs were assinged to critical devices such as Servers and Routers while dynamic IPs via DHCP for end-user devices.
  
   # TechBridge IP addressing documentation
  
# VLAN Configuration Template

| VLAN | Name                     | IP Network     | Subnet Mask     | Devices                                                                                     | Interface / IP Addresses                                           |
|------|--------------------------|----------------|-----------------|---------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| 10   | Domain                   | 192.168.10.0   | 255.255.255.0   | Domain Server, REDC Server, Email Server, Backup Server                                     | S1 G0/2 192.168.10.100<br>S1 F0/10 192.168.10.101<br>S1 F0/11 192.168.10.110<br>S1 F0/12 192.168.10.120 |
| 20   | IT                       | 192.168.20.0   | 255.255.255.0   | PC1, PC2                                                                                   | S1 F0/1-2 192.168.20.11-12                                         |
| 30   | HR                       | 192.168.30.0   | 255.255.255.0   | PC3, PC4                                                                                   | S1 F0/3-4 192.168.30.11-12                                         |
| 40   | Sales                    | 192.168.40.0   | 255.255.255.0   | PC5, PC6                                                                                   | S2 F0/1-2 192.168.40.11-12                                         |
| 50   | Finance                  | 192.168.50.0   | 255.255.255.0   | PC7, PC8                                                                                   | S2 F0/3-4 192.168.50.11-12                                         |
| 60   | Guest Wireless Access Point | 192.168.60.0   | 255.255.255.0   | Access Point                                                                               | S3 F0/1-2 192.168.30.11-12                                         |
| 70   | Guest Computer           | 192.168.70.0   | 255.255.255.0   | PC9, PC10                                                                                  | S3 F0/3-4 192.168.20.11-12                                         |
| 80   | Experience Zone          | 192.168.80.0   | 255.255.255.0   | Minecraft Server, PC11-18                                                                  | S4 F0/1 192.168.80.100<br>S4 F0/2-9 192.168.80.11-20               |
| 99   | Native                   |                |                 | Switch 1, Switch 2, Switch 3, Switch 4                                                     | none                                                               |
| 1000 | Blackhole                | shutdown       | shutdown        | Switch 1, Switch 2, Switch 3, Switch 4                                                     | S1 F0/5-F0/9, F0/13-F0/24<br>S2 F0/5-F0/24<br>S3 F0/5-F0/24<br>S4 F0/10-F0/24 |

---
- ### Network device configuration
--- 
## Router R1
```
Current configuration : 4333 bytes
!
! Last configuration change at 18:16:57 UTC Wed Dec 11 2024
!
version 16.6
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
platform qfp utilization monitor load 80
no platform punt-keepalive disable-kernel-core
!
hostname R1
!
boot-start-marker
boot-end-marker
!
!
vrf definition Mgmt-intf
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
 exit-address-family
!
enable secret 5 $1$gymO$NyujkZH7jlO9W1XiKb31j/
!
no aaa new-model
!
ip domain name techbridge.ca
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.30.1 192.168.30.10
ip dhcp excluded-address 192.168.40.1 192.168.40.10
ip dhcp excluded-address 192.168.50.1 192.168.50.10
ip dhcp excluded-address 192.168.10.1 192.168.10.10
!
ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.254
 dns-server 192.168.10.100
!
ip dhcp pool VLAN30
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.254
 dns-server 192.168.10.100
!
ip dhcp pool VLAN40
 network 192.168.40.0 255.255.255.0
 default-router 192.168.40.254
 dns-server 192.168.10.100
!
ip dhcp pool VLAN50
 network 192.168.50.0 255.255.255.0
 default-router 192.168.50.254
 dns-server 192.168.10.100
!
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.10.100
!
!
!
!
!
!
!
!
!
!
subscriber templating
!
!
!
!
!
!
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
voice-card 0/2
 no watchdog
!
license udi pid ISR4321/K9 sn FDO21082HY4
license boot suite AdvUCSuiteK9
license boot level securityk9
diagnostic bootup level minimal
spanning-tree extend system-id
!
!
!
username admin secret 5 $1$DoNV$u9/hlxAJgcaQ8GMvaf2bp/
!
redundancy
 mode none
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface GigabitEthernet0/0/0
 description R1 connect to Internet
 ip address 10.128.250.222 255.255.255.0
 ip nat outside
 negotiation auto
!
interface GigabitEthernet0/0/1
 description R1 G0/0/1 connect to S1 G0/1
 no ip address
 negotiation auto
!
interface GigabitEthernet0/0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 ip nat inside
!
interface GigabitEthernet0/0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 ip nat inside
 standby 1 ip 192.168.20.254
 standby 1 priority 110
 standby 1 preempt
!
interface GigabitEthernet0/0/1.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
 ip nat inside
 standby 1 ip 192.168.30.254
 standby 1 priority 110
 standby 1 preempt
!
interface GigabitEthernet0/0/1.40
 encapsulation dot1Q 40
 ip address 192.168.40.1 255.255.255.0
 ip nat inside
 standby 1 ip 192.168.40.254
 standby 1 priority 110
 standby 1 preempt
!
interface GigabitEthernet0/0/1.50
 encapsulation dot1Q 50
 ip address 192.168.50.1 255.255.255.0
 ip nat inside
 standby 1 ip 192.168.50.254
 standby 1 priority 110
 standby 1 preempt
!
interface GigabitEthernet0/0/1.99
 encapsulation dot1Q 99 native
!
interface Serial0/1/0
 no ip address
!
interface Serial0/1/1
 no ip address
!
interface Service-Engine0/2/0
!
interface GigabitEthernet0
 vrf forwarding Mgmt-intf
 no ip address
 negotiation auto
!
router ospf 1
 passive-interface GigabitEthernet0/0/0
 network 192.168.0.0 0.0.255.255 area 0
 default-information originate
!
ip nat inside source list 1 interface GigabitEthernet0/0/0 overload
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 GigabitEthernet0/0/0
!
!
access-list 1 permit 172.16.0.0 0.0.255.255
access-list 1 permit 192.168.0.0 0.0.255.255
!
!
!
!
control-plane
!
!
voice-port 0/2/0
!
voice-port 0/2/1
!
voice-port 0/2/2
!
voice-port 0/2/3
!
voice-port 0/2/4
!
voice-port 0/2/5
!
mgcp behavior rsip-range tgcp-only
mgcp behavior comedia-role none
mgcp behavior comedia-check-media-src disable
mgcp behavior comedia-sdp-force disable
!
mgcp profile default
!
!
!
!
!
banner motd ^C Authorized Access Only!! ^C
!
line con 0
 exec-timeout 5 0
 password 7 11081B063743595F50
 login
 transport input none
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
 exec-timeout 5 0
 login local
 transport input ssh
line vty 5 15
 exec-timeout 5 0
 login local
 transport input ssh
!
wsma agent exec
!
wsma agent config
!
wsma agent filesys
!
wsma agent notify
!
!
end

```

 ## Router R2:
```

Current configuration : 3554 bytes
!
! Last configuration change at 22:19:54 UTC Wed Dec 11 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname R2
!
boot-start-marker
boot-end-marker
!
!
security passwords min-length 8
enable secret 5 $1$xi1Q$eV81uyp/kKYehxUR3zcDL0
!
no aaa new-model
!
!
!
!
!
!
!
!
!
!
!
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.30.1 192.168.30.10
ip dhcp excluded-address 192.168.40.1 192.168.40.10
ip dhcp excluded-address 192.168.50.1 192.168.50.10
!
ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.254
 dns-server 192.168.10.100
!
ip dhcp pool VLAN30
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.254
 dns-server 192.168.10.100
!
ip dhcp pool VLAN40
 network 192.168.40.0 255.255.255.0
 default-router 192.168.40.254
 dns-server 192.168.10.100
!
ip dhcp pool VLAN50
 network 192.168.50.0 255.255.255.0
 default-router 192.168.50.254
 dns-server 192.168.10.100
!
!
!
no ip domain lookup
ip domain name techbridge.ca
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
cts logging verbose
!
!
voice-card 0
!
!
!
!
!
!
!
!
license udi pid CISCO2901/K9 sn FJC1925A06P
license boot module c2900 technology-package securityk9
license boot module c2900 technology-package uck9
!
!
!
redundancy
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface Embedded-Service-Engine0/0
 no ip address
 shutdown
!
interface GigabitEthernet0/0
 description R2 G0/0 connect to R3 G0/0
 ip address 172.16.1.1 255.255.255.0
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 description R2 G0/1 connect to S2 G0/1
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.2 255.255.255.0
 standby 1 ip 192.168.20.254
 standby 1 preempt
!
interface GigabitEthernet0/1.30
 encapsulation dot1Q 30
 ip address 192.168.30.2 255.255.255.0
 standby 1 ip 192.168.30.254
 standby 1 preempt
!
interface GigabitEthernet0/1.40
 encapsulation dot1Q 40
 ip address 192.168.40.2 255.255.255.0
 standby 1 ip 192.168.40.254
 standby 1 preempt
!
interface GigabitEthernet0/1.50
 encapsulation dot1Q 50
 ip address 192.168.50.2 255.255.255.0
 standby 1 ip 192.168.50.254
 standby 1 preempt
!
interface GigabitEthernet0/1.99
 encapsulation dot1Q 99 native
!
interface Serial0/0/0
 no ip address
 shutdown
 clock rate 2000000
!
interface Serial0/0/1
 no ip address
 shutdown
 clock rate 2000000
!
router ospf 1
 router-id 2.2.2.2
 network 172.16.1.0 0.0.0.255 area 0
 network 192.168.0.0 0.0.255.255 area 0
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip nat inside source list 2 interface GigabitEthernet0/0 overload
!
!
!
access-list 2 permit 172.16.0.0 0.0.255.255
access-list 2 permit 192.168.0.0 0.0.255.255
!
control-plane
!
 !
 !
 !
 !
!
mgcp behavior rsip-range tgcp-only
mgcp behavior comedia-role none
mgcp behavior comedia-check-media-src disable
mgcp behavior comedia-sdp-force disable
!
mgcp profile default
!
!
!
!
!
!
!
gatekeeper
 shutdown
!
!
banner motd ^C AUTHORIZED ACCESS ONLY!!! ^C
!
line con 0
 password 7 03055908265E731F1A
 logging synchronous
 login
line aux 0
line 2
 no activation-character
 no exec
 transport preferred none
 transport output pad telnet rlogin lapb-ta mop udptn v120 ssh
 stopbits 1
line vty 0 4
 password 7 08204E4D2948574446
 login local
 transport input ssh
line vty 5 15
 password 7 08204E4D2948574446
 login local
 transport input ssh
!
scheduler allocate 20000 1000
!
end
 
```

### Router R3:

```
Current configuration : 2581 bytes
!
! Last configuration change at 21:02:38 UTC Wed Dec 11 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R3
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
memory-size iomem 5
!
!
!
!
!
!
!
!
!
!
!
ip dhcp excluded-address 192.168.60.1 192.168.60.10
ip dhcp excluded-address 192.168.70.1 192.168.70.10
!
ip dhcp pool VLAN60
 network 192.168.60.0 255.255.255.0
 default-router 192.168.60.1
 dns-server 192.168.10.100
!
ip dhcp pool VLAN70
 network 192.168.70.0 255.255.255.0
 default-router 192.168.70.1
 dns-server 192.168.10.100
!
!
!
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
cts logging verbose
!
!
voice-card 0
!
!
!
!
!
!
!
!
license udi pid CISCO2901/K9 sn FJC2008A0S7
license boot module c2900 technology-package securityk9
license boot module c2900 technology-package uck9
!
!
!
redundancy
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface Embedded-Service-Engine0/0
 no ip address
 shutdown
!
interface GigabitEthernet0/0
 description R3 G0/0 connect to R2 G0/0
 ip address 172.16.1.2 255.255.255.0
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 description R3 G0/1 connect to S3 G0/1
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/1.60
 encapsulation dot1Q 60
 ip address 192.168.60.1 255.255.255.0
!
interface GigabitEthernet0/1.70
 encapsulation dot1Q 70
 ip address 192.168.70.1 255.255.255.0
!
interface GigabitEthernet0/1.80
 encapsulation dot1Q 80
 ip address 192.168.80.1 255.255.255.0
!
interface GigabitEthernet0/1.99
 encapsulation dot1Q 99 native
!
interface Serial0/0/0
 no ip address
 shutdown
 clock rate 2000000
!
interface Serial0/0/1
 no ip address
 shutdown
 clock rate 2000000
!
router ospf 1
 network 172.16.1.0 0.0.0.255 area 0
 network 192.168.0.0 0.0.255.255 area 0
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip nat inside source list 3 interface GigabitEthernet0/0 overload
ip route 0.0.0.0 0.0.0.0 GigabitEthernet0/0
!
!
!
access-list 3 permit 192.168.0.0 0.0.255.255
!
control-plane
!
 !
 !
 !
 !
!
mgcp behavior rsip-range tgcp-only
mgcp behavior comedia-role none
mgcp behavior comedia-check-media-src disable
mgcp behavior comedia-sdp-force disable
!
mgcp profile default
!
!
!
!
!
!
!
gatekeeper
 shutdown
!
!
!
line con 0
line aux 0
line 2
 no activation-character
 no exec
 transport preferred none
 transport output pad telnet rlogin lapb-ta mop udptn v120 ssh
 stopbits 1
line vty 0 4
 login
 transport input none
!
scheduler allocate 20000 1000
!
end
```

  ## Switch S1:
```
Current configuration : 6128 bytes
!
! Last configuration change at 02:42:38 UTC Sat Mar 6 1993
!
version 15.2
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname S1
!
boot-start-marker
boot-end-marker
!
enable secret 5 $1$FDC/$ljd.m5ebFilvf4SycJxfX0
!
username admin secret 5 $1$YKaJ$l8lZapOU1Q4AkK9pqa878/
no aaa new-model
system mtu routing 1500
!
!
!
!
!
!
!
no ip domain-lookup
ip domain-name techbridge.ca
login block-for 120 attempts 3 within 60
!
!
crypto pki trustpoint TP-self-signed-747368704
 enrollment selfsigned
 subject-name cn=IOS-Self-Signed-Certificate-747368704
 revocation-check none
 rsakeypair TP-self-signed-747368704
!
!
crypto pki certificate chain TP-self-signed-747368704
 certificate self-signed 01
  30820229 30820192 A0030201 02020101 300D0609 2A864886 F70D0101 05050030
  30312E30 2C060355 04031325 494F532D 53656C66 2D536967 6E65642D 43657274
  69666963 6174652D 37343733 36383730 34301E17 0D393330 33303630 32313731
  315A170D 32303031 30313030 30303030 5A303031 2E302C06 03550403 1325494F
  532D5365 6C662D53 69676E65 642D4365 72746966 69636174 652D3734 37333638
  37303430 819F300D 06092A86 4886F70D 01010105 0003818D 00308189 02818100
  92EB4D2C E969B2A9 0410B26E 4169EC22 434C4F54 521E9693 B34DDD46 180F841B
  9FEE81FB D4C260C6 A5967F8F E771AF87 EB169C40 4682BCF5 A22A247B 94B6AC6A
  EC5F2E14 EE740DC8 661F1BBA 42B71B60 1184B541 F193302D 1974E736 C8B3060D
  178B1EC9 22300D8D 83CF770C E6378FB4 C7B5E1F3 1F30EA32 50BC603D AA0BE355
  02030100 01A35330 51300F06 03551D13 0101FF04 05300301 01FF301F 0603551D
  23041830 168014F6 FE7E8720 D1FB6048 6E687BAF 7CEE464B 73908A30 1D060355
  1D0E0416 0414F6FE 7E8720D1 FB60486E 687BAF7C EE464B73 908A300D 06092A86
  4886F70D 01010505 00038181 00605A46 7703A2A1 555BF3FE C51AE776 9A180AD1
  B9821584 6E09DAFC 4636CC8C 62BA8F97 A38FC300 E4091497 43AFC9E9 AB349CF2
  58A2D4DF 9F976125 51596445 60070F6A 8F634A8A 4AB4BA4A C5259AC4 412E5015
  078B6763 1608903B 9C95C7CC 4EEE750C B2FD3D0F D1629686 8D89DC48 70F6192B
  C83A8E86 C9ED480E 3AFA4245 FF
        quit
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
no cdp run
!
!
!
!
!
!
interface FastEthernet0/1
 description IT Department
 switchport access vlan 20
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 40ed.009f.a73d
 switchport port-security mac-address sticky 8cec.4b83.7bdf
 switchport port-security
!
interface FastEthernet0/2
 description IT Department
 switchport access vlan 20
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky b496.912b.81d3
 switchport port-security
!
interface FastEthernet0/3
 description HR Department
 switchport access vlan 30
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
!
interface FastEthernet0/4
 description HR Department
 switchport access vlan 30
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
!
interface FastEthernet0/5
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/6
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/7
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/8
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/9
 description Domain
 switchport access vlan 10
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 8cec.4b83.7bdf
 switchport port-security
!
interface FastEthernet0/10
 description Domain
 switchport access vlan 10
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 000c.2951.1fb1
 switchport port-security mac-address sticky 74d0.2b26.fd10
 switchport port-security
!
interface FastEthernet0/11
 description Domain
 switchport access vlan 10
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 000c.29f6.ae39
 switchport port-security mac-address sticky 74d0.2b26.fc8e
 switchport port-security
!
interface FastEthernet0/12
 description Domain
 switchport access vlan 10
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
!
interface FastEthernet0/13
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/14
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/15
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/16
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/17
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/18
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/19
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/20
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/21
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/22
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/23
 description S1 F0/23,F0/24 Trunk to S2 F0/23,F0/24
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk
!
interface FastEthernet0/24
 description S1 F0/23,F0/24 Trunk to S2 F0/23,F0/24
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk
!
interface GigabitEthernet0/1
 description S1 G0/1 connect to R1 G0/0/1
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk
!
interface GigabitEthernet0/2
 switchport access vlan 10
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
!
interface Vlan1
 no ip address
!
interface Vlan10
 ip address 192.168.10.3 255.255.255.0
!
interface Vlan20
 ip address 192.168.20.3 255.255.255.0
!
interface Vlan30
 ip address 192.168.30.3 255.255.255.0
!
interface Vlan40
 ip address 192.168.40.3 255.255.255.0
!
interface Vlan50
 ip address 192.168.50.3 255.255.255.0
!
ip http server
ip http secure-server
!
no vstack
banner motd ^CAuthoried Access Only!!^C
!
line con 0
 password 7 104F0B1A2546405858
 login
line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login local
 transport input ssh
!
end
```

  ## Switch S2:
  
```

Current configuration : 6128 bytes
!
! Last configuration change at 03:26:32 UTC Sat Mar 6 1993
!
version 15.2
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname S2
!
boot-start-marker
boot-end-marker
!
enable secret 5 $1$zCXD$lDeU9kCeeqgS9WSuJO5Xh0
!
username admin secret 5 $1$dKJ6$mInr1TY4EhTwCqxs276EV0
no aaa new-model
system mtu routing 1500
!
!
!
!
!
!
!
login block-for 120 attempts 3 within 60
!
!
crypto pki trustpoint TP-self-signed-3509153920
 enrollment selfsigned
 subject-name cn=IOS-Self-Signed-Certificate-3509153920
 revocation-check none
 rsakeypair TP-self-signed-3509153920
!
!
crypto pki certificate chain TP-self-signed-3509153920
 certificate self-signed 01
  3082022B 30820194 A0030201 02020101 300D0609 2A864886 F70D0101 05050030
  31312F30 2D060355 04031326 494F532D 53656C66 2D536967 6E65642D 43657274
  69666963 6174652D 33353039 31353339 3230301E 170D3933 30333036 30333234
  35335A17 0D323030 31303130 30303030 305A3031 312F302D 06035504 03132649
  4F532D53 656C662D 5369676E 65642D43 65727469 66696361 74652D33 35303931
  35333932 3030819F 300D0609 2A864886 F70D0101 01050003 818D0030 81890281
  810089CB 6C411CD3 601FA64A D3E86AAA 56B6B10F 01CCDED3 BBFCAC44 01576AE5
  47EBDD33 7B287597 E8109504 46B66EAF A93882D1 CEEC0606 039EF52B 15F01587
  FB677137 CA24081C E0D5AF1C CE034A5C 09DA425B 5D52AE0C DBB3AA6E 25A4AD2A
  16F64664 9EAF8C1D BF573DDE 9F3298D3 BEF689C1 032190E5 A182C76D D5668421
  F0CB0203 010001A3 53305130 0F060355 1D130101 FF040530 030101FF 301F0603
  551D2304 18301680 146D81E5 1625CCED 75E53202 4927534A F5DA42C7 2E301D06
  03551D0E 04160414 6D81E516 25CCED75 E5320249 27534AF5 DA42C72E 300D0609
  2A864886 F70D0101 05050003 81810015 79EC5290 AC802F30 19398E4E F82D9057
  84B4D822 498FDFFC CDF1247A 99CE6C64 8D5014C4 8B4764B6 105FE1CF B99AB3AC
  5B28E428 1A3630F2 C1E25984 65071C4E 8C5378D9 8D966A30 F858A179 A2C2016D
  34101B52 EB0B174F 0842E78F 3CB268E0 07E61D02 E9716503 1DF80A8D 81804B4E
  0E404B1F 04996CC1 289E0ED5 AA9E7B
        quit
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
no cdp run
!
!
!
!
!
!
interface FastEthernet0/1
 description Sale Department
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
!
interface FastEthernet0/2
 description Sale Department
 switchport access vlan 40
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
!
interface FastEthernet0/3
 description Finance Department
 switchport access vlan 50
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
!
interface FastEthernet0/4
 description Finance Department
 switchport access vlan 50
 switchport mode access
 switchport port-security maximum 2
 switchport port-security violation  restrict
 switchport port-security mac-address sticky
 switchport port-security
!
interface FastEthernet0/5
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/6
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/7
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/8
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/9
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/10
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/11
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/12
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/13
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/14
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/15
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/16
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/17
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/18
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/19
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/20
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/21
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/22
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/23
 description S2 F0/23,F0/24 trunk to S1 F0/23,F0/24
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk
!
interface FastEthernet0/24
 description S2 F0/23,F0/24 trunk to S1 F0/23,F0/24
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk
!
interface GigabitEthernet0/1
 description S2 G0/1 connect to R2 G0/1
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,40,50,99
 switchport mode trunk
!
interface GigabitEthernet0/2
 switchport access vlan 1000
 switchport trunk allowed vlan 20,30,40,50
 switchport mode access
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan10
 ip address 192.168.10.4 255.255.255.0
!
interface Vlan20
 ip address 192.168.20.4 255.255.255.0
!
interface Vlan30
 ip address 192.168.30.4 255.255.255.0
!
interface Vlan40
 ip address 192.168.40.4 255.255.255.0
!
interface Vlan50
 ip address 192.168.50.4 255.255.255.0
!
ip http server
ip http secure-server
!
no vstack
banner motd ^C Authorized Access Only!!^C
!
line con 0
 password 7 03055908265E731F1A
 login
line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login local
 transport input ssh
!
end
```
## Switch S3:

```
Current configuration : 6274 bytes
!
! Last configuration change at 02:02:14 UTC Tue Mar 2 1993
!
version 15.0
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname S3
!
boot-start-marker
boot-end-marker
!
enable secret 5 $1$radD$mtrSYoCcND8TgzkiAb/BQ0
!
username admin secret 5 $1$1DEU$2zU2cVcXOLqgqBKPWcvPC.
no aaa new-model
system mtu routing 1500
!
!
no ip domain-lookup
ip domain-name techbridge.ca
login block-for 120 attempts 3 within 60
!
!
crypto pki trustpoint TP-self-signed-1364513664
 enrollment selfsigned
 subject-name cn=IOS-Self-Signed-Certificate-1364513664
 revocation-check none
 rsakeypair TP-self-signed-1364513664
!
!
crypto pki certificate chain TP-self-signed-1364513664
 certificate self-signed 01
  3082022B 30820194 A0030201 02020101 300D0609 2A864886 F70D0101 05050030
  31312F30 2D060355 04031326 494F532D 53656C66 2D536967 6E65642D 43657274
  69666963 6174652D 31333634 35313336 3634301E 170D3933 30333031 30323036
  32325A17 0D323030 31303130 30303030 305A3031 312F302D 06035504 03132649
  4F532D53 656C662D 5369676E 65642D43 65727469 66696361 74652D31 33363435
  31333636 3430819F 300D0609 2A864886 F70D0101 01050003 818D0030 81890281
  8100DFD0 83CA2A73 E3E483DB FD537FCE 0F00D668 816561DD D08C5EA7 B79BF97F
  44368CF8 387A9B6D 9B13AF9B 75304442 BCE968E4 6BC3DE44 5F72CB7E 8885F2E1
  754959ED FBDB4DB9 7E96F40C C30AE831 83BB0F58 F527BB83 F93A9DAC C660A710
  BA995649 F781F2D9 B3FE1FE9 DBD5E3F0 36784735 6D50768F FD9F5339 1C709A77
  1BB10203 010001A3 53305130 0F060355 1D130101 FF040530 030101FF 301F0603
  551D2304 18301680 14642F91 29EB6929 310AAEB0 3AE3009D A091BC71 8A301D06
  03551D0E 04160414 642F9129 EB692931 0AAEB03A E3009DA0 91BC718A 300D0609
  2A864886 F70D0101 05050003 8181005B 6325CDAD C8E04CE9 0F9EF7A3 1C36D4BD
  69A19376 E596876D DC35B741 8B67CBDA E210FB9A A3BA7C6A AB597F6E 85F9DD9E
  821B9800 8FDA9A51 6E74181F 270D1468 4FB22E10 66F7362B 88E04217 2D9A696E
  01A8973F EE3C74F7 6729CB82 6CC8CB23 E55224B9 F9B0A0D7 C7DA97BA FC4B4683
  23C52433 403A8F58 7B6D98C9 159290
        quit
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
!
!
!
!
!
interface FastEthernet0/1
 description Guest Wireless Zone
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/2
 description Guest Wireless Zone
 switchport access vlan 60
 switchport mode access
!
interface FastEthernet0/3
 description Guest Zone
 switchport access vlan 70
 switchport mode access
!
interface FastEthernet0/4
 description Guest Zone
 switchport access vlan 70
 switchport mode access
!
interface FastEthernet0/5
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/6
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/7
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/8
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/9
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/10
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/11
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/12
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/13
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/14
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/15
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/16
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/17
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/18
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/19
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/20
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/21
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/22
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/23
 description S3 F0/23-24 trunk to S4 F0/23-24
 switchport trunk native vlan 99
 switchport trunk allowed vlan 60,70,80,99
 switchport mode trunk
!
interface FastEthernet0/24
 description S3 F0/23-24 trunk to S4 F0/23-24
 switchport trunk native vlan 99
 switchport trunk allowed vlan 60,70,80,99
 switchport mode trunk
!
interface GigabitEthernet0/1
 description S3 G0/1 connect to R3 G0/1
 switchport trunk native vlan 99
 switchport trunk allowed vlan 60,70,80,99
 switchport mode trunk
 ip access-group 102 in
!
interface GigabitEthernet0/2
 switchport access vlan 1000
 switchport mode access
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan60
 ip address 192.168.60.3 255.255.255.0
!
interface Vlan70
 ip address 192.168.70.3 255.255.255.0
!
interface Vlan80
 ip address 192.168.80.3 255.255.255.0
!
ip http server
ip http secure-server
!
access-list 102 deny   ip 192.168.60.0 0.0.0.255 192.168.20.0 0.0.0.255
access-list 102 deny   ip 192.168.60.0 0.0.0.255 192.168.30.0 0.0.0.255
access-list 102 deny   ip 192.168.60.0 0.0.0.255 192.168.40.0 0.0.0.255
access-list 102 deny   ip 192.168.60.0 0.0.0.255 192.168.50.0 0.0.0.255
access-list 102 deny   ip 192.168.70.0 0.0.0.255 192.168.20.0 0.0.0.255
access-list 102 deny   ip 192.168.70.0 0.0.0.255 192.168.30.0 0.0.0.255
access-list 102 deny   ip 192.168.70.0 0.0.0.255 192.168.40.0 0.0.0.255
access-list 102 deny   ip 192.168.70.0 0.0.0.255 192.168.50.0 0.0.0.255
access-list 102 deny   ip 192.168.80.0 0.0.0.255 192.168.20.0 0.0.0.255
access-list 102 deny   ip 192.168.80.0 0.0.0.255 192.168.30.0 0.0.0.255
access-list 102 deny   ip 192.168.80.0 0.0.0.255 192.168.40.0 0.0.0.255
access-list 102 deny   ip 192.168.80.0 0.0.0.255 192.168.50.0 0.0.0.255
access-list 102 permit ip any any
no cdp run
!
banner motd ^C AUTHORIZED ACCESS ONLY!!! ^C
!
line con 0
 password 7 094D4C0A395445415F
 login
line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login local
 transport input ssh
!
end
```
## Switch S4
```
Current configuration : 5435 bytes
!
! Last configuration change at 22:20:58 UTC Mon Mar 1 1993
!
version 15.0
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname S4
!
boot-start-marker
boot-end-marker
!
enable secret 5 $1$tOuA$5H66tQ4Ba5AD0OT53WOML0
!
username admin secret 5 $1$z8qH$KCujWVngyFWaVgp.LQmnl1
no aaa new-model
system mtu routing 1500
!
!
 --More--
no ip domain-lookup
ip domain-name techbridge.ca
login block-for 120 attempts 3 within 60
!
!
crypto pki trustpoint TP-self-signed-2366598912
 enrollment selfsigned
 subject-name cn=IOS-Self-Signed-Certificate-2366598912
 revocation-check none
 rsakeypair TP-self-signed-2366598912
!
!
crypto pki certificate chain TP-self-signed-2366598912
 certificate self-signed 01
  3082022B 30820194 A0030201 02020101 300D0609 2A864886 F70D0101 05050030
  31312F30 2D060355 04031326 494F532D 53656C66 2D536967 6E65642D 43657274
  69666963 6174652D 32333636 35393839 3132301E 170D3933 30333036 32323134
  32385A17 0D323030 31303130 30303030 305A3031 312F302D 06035504 03132649
  4F532D53 656C662D 5369676E 65642D43 65727469 66696361 74652D32 33363635
  39383931 3230819F 300D0609 2A864886 F70D0101 01050003 818D0030 81890281
  8100B6D5 FED96939 BBF37C93 37098558 38F1275B 5E08EEC2 7A16D4BA 55A60F21
  6FA7DA4A 7C18DF55 DB154640 FAFBBB6C 829AB478 7397B9A4 DC3551C0 2AA29FF4
  D320B045 D3A04FEC 1BA5A22A D2522118 5DB7FFBB 7B254DC3 5C9B6FFE C66D7026
  D4F2D138 0B50B96C 0E353FBB F86C84C8 3F2123B1 AD26146C 668EE30B 9F1A2E56
  AC0F0203 010001A3 53305130 0F060355 1D130101 FF040530 030101FF 301F0603
  551D2304 18301680 1488F882 14E92F2F CE7A750F 574F6465 40610323 EF301D06
  03551D0E 04160414 88F88214 E92F2FCE 7A750F57 4F646540 610323EF 300D0609
  2A864886 F70D0101 05050003 81810026 1577DCA2 95FEADEC 7672C534 48A5FB3C
  A04B7121 DD038845 6A5017E5 02492D04 6D42D09A 5D2A2B3D 2F503C37 E938AE95
  F1FE23EF 79513A62 3A78A856 87BA7E48 9607B676 0FB2081B 30B50C62 33284704
  3F498609 88E3E841 CAA9D5BD 78B2FADB 7AB5593C 2B5FC792 F45C8413 8E521CA2
  63AA8225 821D8EED EC211287 87EA4F
        quit
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
!
!
!
!
!
interface FastEthernet0/1
 description Experience Zone
 switchport access vlan 80
 switchport mode access
!
interface FastEthernet0/2
 description Experience Zone
 switchport access vlan 80
 switchport mode access
!
interface FastEthernet0/3
 description Experience Zone
 switchport access vlan 80
 switchport mode access
!
interface FastEthernet0/4
 description Experience Zone
 switchport access vlan 80
 switchport mode access
!
interface FastEthernet0/5
 description Experience Zone
 switchport access vlan 80
 switchport mode access
!
interface FastEthernet0/6
 description Experience Zone
 switchport access vlan 80
 switchport mode access
!
interface FastEthernet0/7
 description Experience Zone
 switchport access vlan 80
 switchport mode access
!
interface FastEthernet0/8
 description Experience Zone
 switchport access vlan 80
 switchport mode access
!
interface FastEthernet0/9
 description Experience Zone
 switchport access vlan 80
 switchport mode access
!
interface FastEthernet0/10
 description Experience Zone
 switchport access vlan 80
 switchport mode access
!
interface FastEthernet0/11
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/12
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/13
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/14
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/15
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/16
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/17
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/18
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/19
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/20
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/21
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/22
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface FastEthernet0/23
 description S4 F0/23,F0/24 trunk to S3 F0/23,F0/24
 switchport trunk native vlan 99
 switchport trunk allowed vlan 60,70,80,99
 switchport mode trunk
!
interface FastEthernet0/24
 description S4 F0/23,F0/24 trunk to S3 F0/23,F0/24
 switchport trunk native vlan 99
 switchport trunk allowed vlan 60,70,80,99
 switchport mode trunk
!
interface GigabitEthernet0/1
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface GigabitEthernet0/2
 switchport access vlan 1000
 switchport mode access
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan80
 ip address 192.168.80.4 255.255.255.0
!
ip default-gateway 192.168.80.1
ip http server
ip http secure-server
!
no cdp run
!
banner motd ^C AUTHORIZED ACCESS ONLY!!! ^C
!
line con 0
 password 7 03055908265E731F1A
 logging synchronous
 login
line vty 0 4
 password 7 094D4C0A395445415F18162B2537383C2736530E18131442194B405C
 login local
line vty 5 15
 password 7 094D4C0A395445415F18162B2537383C2736530E18131442194B405C
 login local
!
end
```


### VLANs and Segmentation 
- Vlan was implemented to segment traffic, minimize risks, enforce policies, and ensure that critical systems remain secure and operational.
- VLANs was created for separating traffic based on departments (e.g.,Domain vlan 10, IT vlan 20, HR Vlan 30, Finance vlan 40 and gaming exhibition zone has vlan 60,70 and 80).  
- Inter-VLAN routing will be configured on Routers 1, 2 and 3.  


### Wireless Network  
- Deploy access points with dual SSIDs:  
  - Employee network which is the Techbrige network uses (secure, WPA3).  
  - Guest network which is the Open house Network is isolated from business-critical systems.  

---

## 2. Server and Services  

### Objectives  
- Deploy centralized server infrastructure to support core business operations.  
- Ensure redundancy and scalability for key services of the **open house exhibition**.  

### Server Deployment  
**Game Server** 
- OS: Ubuntu 24.04.1 LTS(Server) 
- Web Console: 
-	Version: cockpit 
- mcserver:9090 (192.168.80.100:9090) 
- Description: Let Administrator remote control the ubuntu server 
- Reference: Cockpit Official 
- **Monitor Tool:** 
	- Version:Bashtop 
	- Function Start prompt: bashtop 
	- Leave Function: Esc then select quit 
	- Description: Provide more visualize monitoring on ubuntu 
- Reference: Bashtop Github 
**Game: Minecraft** 
- Version:1.21.4 
- Java version: OpenJDK 21 
- Network Setting: IP address 192.168.80.100 Port:25565 
- Server Start Prompt: java -Xms1G -Xmx1G -jar server.jar --nogui 
- Server Stop Prompt: /stop 
- **How to Join server:**  
- Step 1.) Open Minecraft Lanuch 
- Step 2.) Login your account 
- Step 3.) Choose game version [ 1.21.4 ] 
- Step 4.) Click “PLAY” Green Bottom 
- Step 5.) Select [Multiplayer] 
- Step 6.) Select [Direct Connection] or [Add Server] 
- Step 7.) At Server Address, type [ 192.168.80.100:25565 ]  then Click [Join Server] 
- Step 8.) Enjoy your Game Time ! 
- Description: Run a unique Minecraft multiplayer server. 
- Reference:  
- Minecraft Official Site 
- Minecraft Wiki 
 
**DHCP Server** 
- OS: Ubuntu 24.04.1 LTS (Server) 
- Version: isc-dhcp-server 
- DHCP Configure: 
- range 192.168.80.11 192.168.80.20; 
- routers 192.168.80.1; 
- domain-name-servers 192.168.10.100; 
- domain-name "techbridge.ca"; 
- **Description:** Let Experience Zone PC get IP addresses, range between 192.168.80.11~192.168.80.20  
- Reference: 
- Ubuntu Server 
- Isc-dhcp-server install 
 
**Web Server** 
- OS: Ubuntu 24.04.1 LTS (Server) 
- Version: Apache 2 
- Web Addresses: openhouse.techbridge.ca (192.168.80.100) 
- Description: Design, storage and execute a webpage for welcome the player on experience zone PC  
- Reference: 
- Apache2 
- Apache2 Setting 
 
**Domain controller Server (Main)** 
- OS: Windows server 2022 
- Version: Windows Server 2022 Datacenter Evaluation (Build 20348) 
- Network Setting: IP address 192.168.10.100/24 
- Remote Assistance tool: 
- Service provider: Windows server 2022 
- **Description:** Support on-time remote assistance without account log out. 
- **Description:** Server that processes requests for authentication from users within the domain TechBridge.ca, including users, authentication credentials and enterprise security policies. 
 
**Redundant Domain controller Server**  
- OS: Windows server 2022 
- Version: Windows Server 2022 Datacenter Evaluation (Build 20348) 
- Network Setting: IP address 192.168.10.101/24 
- Description: Redundant domain controllers can ensure high availability of Active Directory services even if main domain controller goes down. 
 
**Exchange Server (Email server)** 
- OS: Windows server 2022 
- Version: Windows Server 2022 Datacenter Evaluation (Build 20348) 
- Network Setting: IP address 192.168.10.110/24 
- **Description:** Exchange Server includes features like mailbox management, collaboration tools, and security protocols that improve productivity and simplify team communication in TechBridge company.   
 
**Backup Server**  
- OS: Windows server 2022 
- Version: Windows Server 2022 Datacenter Evaluation (Build 20348) 
- Network Setting: IP address 192.168.10.120/24 
- Description: Backup TechBridge company’s main DC server, exchange server and redundant DC server. 
 
**Helpdesk ticket system** 
- App name: ServiceDesk Plus Cloud 
- Service provider: Zoho Corporation 
- Description: Support Help Desk ticket system and in-team help desk support. Also support on-time assistance. 
 
---

## 3. Security Hardening for Routers and Switches
- Because the Open house gaming exhibition will be hosted on techbridge building and using some of the techbridge resoures and network it essential that our network is secured and protected to :
  1. Minimize vulnerabilities and ensures a secure network environment. 
  2. Protect network infrastructure, servers, and endpoints from unauthorized access.  
  3. Ensure compliance with industry security standards.  
### Switch hardening
- The following security measures will be implemented on all switches on the network
  1. Secure Management Access: Includes enabling SSH and disabling Telnet for encrypted communication.
  2. Restrict Console and VTY Access by Limit access to trusted IPs only.
  3. Set complex passwords for all access levels.
  4. Disable all services such as, cdp, and lldp.
  5. Enable Port Security by restricting MAC addresses on ports to prevent unauthorized devices.
  6. Implement VLAN for security and to seperate network trafic.
  7. Disable all the unused Ports.
  8. Regular maintenance and updates by keeping the OS updated and backing up configurations.
  
### Router hardening
- Securing a Cisco router is critical to protect your network against unauthorized access, attacks, and vulnerabilities.
  1. Control access to the router by enabling SSH and setting strong password.
  2. Restrict console and line VTY access.
  3. Implement ACL's to only permit access from a trusted network.
  4. Disable unused services such as, cdp, lldp, http.
  5. Disable all unused interface.
  6. Regular maintenance and updates by keeping the OS updated and backing up configurations.

  ### End device hardening
   1. Operating System Security: Enable automatic updates for Windows, and all other OS connected to the network.
   2. Implement multi-factor authentication (MFA) as an additional layer of security.
   3. Use screen-lock time out.
   4. Security Awareness training: To educate users about phishing, malware, and social engineering attacks.
      
### Securing Protocols
   The following security protocols were implemented on the network.
   - Use HTTPS over HTTP
   - Using SSH instead of Telnet for remote network.
   - Secure routing protocols like OSPF.
     
### Secure Access  
- **Authentication:**  
  - Enforce multi-factor authentication (MFA) for all users.  
  - Implement role-based access control (RBAC).     

### Regular Assessments  
- Conduct routine vulnerability scans and penetration tests.  
- Maintain a compliance log and audit trail for network changes.  

---

## Budget and Timeline  
 
# Budget Estimate

| **Category**            | **Estimated Cost (USD)** |
|--------------------------|--------------------------|
| Venue Rental            | $5000                 |
| Equipment and Setup     | $20,000                 |
| Marketing and Promotion | $7,000                |
| Prizes and Giveaways    | $100,000                 |
| Staffing and Security   | $56,000                 |
| Miscellaneous           | $15,000                 |
| **Total**               | **$203,000**            |

- **Hardware requirements for the Open house gaming exhibition:**  
  - 1 Router
  - 2 switches
  - 1 servers
  - 1 access point
  - 12 Computers  
 
### Timeline 
#### Planning Phase (1-2 days)
 - Define the goals and objectives of the network.
 - Identify required features (e.g., gaming servers, live streaming, player leaderboards).
 - Decide on hardware and software specifications.
 - Create a network design (e.g., LAN/WAN setup, number of nodes, internet bandwidth).
#### Procurement and Setup (3 days)
 - Acquire hardware (servers, gaming rigs, routers, switches, and cables)
 - Install and configure network infrastructure (cabling, Wi-Fi, switches).
 - Set up gaming platforms and test compatibility.
#### 3. Software Configuration (2 days)
 - Install gaming and management software (e.g., server management tools, anti-cheat systems).
 - Set up game servers and user accounts.
 - Configure firewalls, VPNs, and other security measures.
#### 4. Testing and Troubleshooting (3 day)
  - Perform network stress tests and resolve bottlenecks.
  - Test gaming server performance under different loads.
  - Fix bugs or connectivity issues.
#### 5. Presentation (1 day)

- **Team size** 4 Team members.
- Total Estimated Time: (9-10 days)


## Conclusion  
This proposal outlines a comprehensive network plan and designed to support the growth and operational efficiency of Techbridge open house gaming exhibition. Our recommendations ensure a secure, scalable, and highly available infrastructure tailored to meet the business needs of our Open House gaming exhibition.  

**Prepared by:**  
Mike Ogunyemi, Kirti Sharma, Hu Ching-Chuan, Hu Ching-An  
130 Henlow Bay.
Winnipeg, MB, R3Y 1G4.
info@techbridge.ca  
December 12, 2024 
