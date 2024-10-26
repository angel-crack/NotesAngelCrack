[+] show running-config
 
```c
Current configuration : 14427 bytes
!
! Last configuration change at 12:28:19 EST Mon Oct 21 2024 by cisco
! NVRAM config last updated at 12:28:22 EST Mon Oct 21 2024 by cisco
!
version 15.6
service timestamps debug datetime msec
service timestamps log datetime localtime show-timezone
no service password-encryption
!
hostname COMM-CUCM
!
boot-start-marker
boot system flash:c2900-universalk9-mz.SPA.156-3.M1.bin
boot-end-marker
!
!
logging buffered 512000
enable password cisco
!         
aaa new-model
!
!
aaa authentication login default local
aaa authentication login VTY local
aaa authentication login console none
!
!
!
!
!
aaa session-id common
clock timezone EST -5 0
!
!
!
!
!
!
!         
ip traffic-export profile OMAR mode capture
  bidirectional
  incoming access-list CAPTURE
  outgoing access-list CAPTURE
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
ip vrf Management
!
ip dhcp excluded-address 10.255.255.1 10.255.255.50
!
!
!
no ip domain lookup
ip domain name cucm.cotac.com
ip name-server vrf CUCM 192.168.110.122
ip name-server vrf CUCM 8.8.8.8
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
cts logging verbose
!
crypto pki server iosca
 database level complete
 no database archive
 grant auto
 lifetime certificate 1800
 shutdown
!
crypto pki trustpoint TP-self-signed-3772747110
 enrollment selfsigned
 subject-name cn=IOS-Self-Signed-Certificate-3772747110
 revocation-check none
 rsakeypair TP-self-signed-3772747110
!
crypto pki trustpoint iosca
 enrollment url http://192.168.110.1:80
 revocation-check none
 rsakeypair iosca
!
!
crypto pki certificate chain TP-self-signed-3772747110
 certificate self-signed 01
  3082022B 30820194 A0030201 02020101 300D0609 2A864886 F70D0101 05050030 
  31312F30 2D060355 04031326 494F532D 53656C66 2D536967 6E65642D 43657274 
  69666963 6174652D 33373732 37343731 3130301E 170D3134 31323138 32333532 
  35395A17 0D323030 31303130 30303030 305A3031 312F302D 06035504 03132649 
  4F532D53 656C662D 5369676E 65642D43 65727469 66696361 74652D33 37373237 
  34373131 3030819F 300D0609 2A864886 F70D0101 01050003 818D0030 81890281 
  81009A93 243F169F E49E0C33 04494DB6 94C70C46 ED5E1DD4 8314C94E 51415626 
  C815F61F A89DE815 BBCFC56C F16A82DE 83A6F15A B155422F D9017E8D 70D67FD1 
  CFC1041C B8E60424 B91D0CEF E3E2232F B920B53F 42CCC09B 0929A499 58EC655D 
  63650A19 E87B016A ADA9BB5A 4673EB6C F92FACFD 5A66F3D7 6A5D9340 5CD1908A 
  66510203 010001A3 53305130 0F060355 1D130101 FF040530 030101FF 301F0603 
  551D2304 18301680 14766CDA C20D6932 429F658F 60365D45 60095C60 1E301D06 
  03551D0E 04160414 766CDAC2 0D693242 9F658F60 365D4560 095C601E 300D0609 
  2A864886 F70D0101 05050003 81810040 AB157A94 2DD44B33 D5F48F1C 03F8C688 
  0CC9726E 4C77699A 23428DDB FE7A9192 EA065638 A37A9BF0 7EDEED18 9B58DD5D 
  08BDA3A3 EA936906 EC5659CE CA7A421F 8D1C5218 050B7663 E71E02EA 25D12023 
  C2332CE7 CE4532F6 141C189F E840FEF5 557C8006 3191F50F DA113741 33FAB895 
  55FD3E39 DECF53A5 E15F381D E7C1EF
        quit
crypto pki certificate chain iosca
 certificate ca 01
  308201F9 30820162 A0030201 02020101 300D0609 2A864886 F70D0101 04050030 
  10310E30 0C060355 04031305 696F7363 61301E17 0D323130 35323131 37303934 
  325A170D 32343035 32303137 30393432 5A301031 0E300C06 03550403 1305696F 
  73636130 819F300D 06092A86 4886F70D 01010105 0003818D 00308189 02818100 
  BC491380 98740435 D2B9163B 545251FF 21C3F774 5D8F85A7 9880DD93 098CF3F6 
  57DCC833 1167AA2D 9F524DBE 4CCD485C B90E4798 6F6BCC16 A957695C 1303CFD8 
  A18E3E12 86476E64 002ED1F2 C406130B 070E9EC5 F5CFA1DF 24539850 4CFE9109 
  22D45FE3 434D64EC 17F0A523 A1206614 7CB38B1F C00C12F1 BC4809A8 17BDC553 
  02030100 01A36330 61300F06 03551D13 0101FF04 05300301 01FF300E 0603551D 
  0F0101FF 04040302 0186301F 0603551D 23041830 16801476 CF406553 33BB5BE1 
  62914932 5964F9E9 E66F1A30 1D060355 1D0E0416 041476CF 40655333 BB5BE162 
  91493259 64F9E9E6 6F1A300D 06092A86 4886F70D 01010405 00038181 0017465E 
  F6080ABC 1D91E87E E13433ED 8672D23A 1E75E0DA E8DCB700 5125883F 5B3B7E24 
  AA365036 D49141B9 C73476C2 8446EE9A F7837854 E52A1489 59E600FB A3A87DDD 
  F1AE3D46 EBA656DB A3EB5F92 FC3BC003 14B2C09D EB1D45FC 99F3D354 AE1DC325 
  E6341F8D B8B7CFFD D41650CA 36B01285 4EE78FB7 41DEA86B CEADE98E 18
        quit
license udi pid CISCO2901/K9 sn FGL185122WN
license accept end user agreement
license boot module c2900 technology-package securityk9
!
!         
username cisco privilege 15 secret 5 $1$STac$saHxq0dGaT3dX3xJgZgWw1
username training privilege 15 secret 5 $1$SiUq$DNDGGdl4AUudQz3z7XrVM0
!
redundancy
!
!
!
!
!
ip tcp selective-ack
!
class-map match-any SIGNAL
 match ip dscp cs3 
class-map match-any VOICE
 match ip dscp ef 
!
policy-map WAN-QUEUING
 class VOICE
  priority percent 30
 class SIGNAL
  bandwidth remaining percent 10 
 class class-default
  bandwidth remaining percent 90 
policy-map WAN-QOS
 class class-default
  shape average 2000000   
   service-policy WAN-QUEUING
!
! 
crypto keyring CUCM-keyring  
  pre-shared-key address 10.168.105.162 key CUCM123
!
crypto isakmp policy 1
 encr aes
 hash sha256
 authentication pre-share
 group 14
crypto isakmp profile CUCM-isakmp-pro
   keyring CUCM-keyring
   match identity address 10.168.105.162 255.255.255.255 
!
!
crypto ipsec transform-set Jeison-tset esp-aes 256 esp-sha256-hmac 
 mode transport
!
crypto ipsec profile CUCM-Pro
 set security-association lifetime seconds 36800
 set transform-set Jeison-tset 
 set isakmp-profile CUCM-isakmp-pro
!
!
!
!
!
!
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
!
interface Loopback1
 ip address 10.225.139.33 255.255.255.224
 ip nat outside
 ip virtual-reassembly in
!
interface Tunnel1
 ip address 192.168.200.2 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
 keepalive 5 1
 tunnel source 10.168.110.66
 tunnel destination 10.168.105.162
!
interface Embedded-Service-Engine0/0
 no ip address
 shutdown
!
interface GigabitEthernet0/0
 description Management - Internet
 no ip address
 ip nat outside
 ip virtual-reassembly in
 duplex auto
 speed auto
!
interface GigabitEthernet0/0.180
 encapsulation dot1Q 180 native
 ip address 10.168.110.66 255.255.255.192
 ip nat outside
 ip virtual-reassembly in
!
interface GigabitEthernet0/0.777
 encapsulation dot1Q 777
 ip nat outside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1
 no ip address
 duplex auto
 speed auto
!         
interface GigabitEthernet0/1.700
 description TO CUCM-SW1 CUCM Lab
 encapsulation dot1Q 700
 ip address 192.168.110.1 255.255.255.0
 ip pim dense-mode
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.701
 description Voice VLAN
 encapsulation dot1Q 701
 ip address 192.168.255.1 255.255.255.0
 ip helper-address 192.168.110.122
 ip pim sparse-dense-mode
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.702
 description Wireless Vlan
 encapsulation dot1Q 702
 ip address 192.168.254.1 255.255.255.0
 ip helper-address 192.168.110.122
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.703
 description Focus Group1
 encapsulation dot1Q 703
 ip address 192.168.10.1 255.255.255.0
 ip access-group BloqueaBobos in
 ip pim sparse-dense-mode
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.704
 description Focus Group2
 encapsulation dot1Q 704
 ip address 192.168.20.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.705
 description Focus Group3
 encapsulation dot1Q 705
 ip address 192.168.30.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.710
 description CUCM Device Mobility
 encapsulation dot1Q 710
 ip address 172.168.110.1 255.255.255.0
 ip helper-address 192.168.110.122
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.800
 description UCS-E VLAN 800
 encapsulation dot1Q 800
 ip address 192.168.40.1 255.255.255.0
 ip access-group BloqueaBobos in
 ip pim sparse-dense-mode
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.810
 description UCS-E VLAN 810
 encapsulation dot1Q 810
 ip address 192.168.50.1 255.255.255.0
 ip access-group BloqueaBobos in
 ip pim sparse-dense-mode
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.820
 description UCS-E VLAN 820
 encapsulation dot1Q 820
 ip address 192.168.60.1 255.255.255.0
 ip access-group BloqueaBobos in
 ip pim sparse-dense-mode
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.830
 description UCS-E VLAN 830
 encapsulation dot1Q 830
 ip address 192.168.70.1 255.255.255.0
 ip access-group BloqueaBobos in
 ip pim sparse-dense-mode
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.840
 description UCS-E VLAN 840
 encapsulation dot1Q 840
 ip address 192.168.80.1 255.255.255.0
 ip access-group BloqueaBobos in
 ip pim sparse-dense-mode
 ip nat inside
 ip virtual-reassembly in
!         
interface GigabitEthernet0/1.850
 description UCS-E VLAN 850
 encapsulation dot1Q 850
 ip address 192.168.90.1 255.255.255.0
 ip access-group BloqueaBobos in
 ip pim sparse-dense-mode
 ip nat inside
 ip virtual-reassembly in
!
interface GigabitEthernet0/1.860
 description UCS-E VLAN 860
 encapsulation dot1Q 860
 ip address 192.168.100.1 255.255.255.0
 ip access-group BloqueaBobos in
 ip pim sparse-dense-mode
 ip nat inside
 ip virtual-reassembly in
!
interface Async0/0/0
 no ip address
 encapsulation slip
!
interface Async0/0/1
 no ip address
 encapsulation slip
!
interface Async0/0/2
 no ip address
 encapsulation slip
!
interface Async0/0/3
 no ip address
 encapsulation slip
!
interface Async0/0/4
 no ip address
 encapsulation slip
!
interface Async0/0/5
 no ip address
 encapsulation slip
!
interface Async0/0/6
 no ip address
 encapsulation slip
!
interface Async0/0/7
 no ip address
 encapsulation slip
!
interface Async0/0/8
 no ip address
 encapsulation slip
!
interface Async0/0/9
 no ip address
 encapsulation slip
!
interface Async0/0/10
 no ip address
 encapsulation slip
!
interface Async0/0/11
 no ip address
 encapsulation slip
!
interface Async0/0/12
 no ip address
 encapsulation slip
!
interface Async0/0/13
 no ip address
 encapsulation slip
!
interface Async0/0/14
 no ip address
 encapsulation slip
!
interface Async0/0/15
 no ip address
 encapsulation slip
!
!
ip forward-protocol nd
!
ip http server
no ip http secure-server
!
ip tftp source-interface GigabitEthernet0/1.700
ip dns view vrf CUCM default
ip nat translation timeout 55555
ip nat inside source static 192.168.110.136 10.168.110.67
ip nat inside source static 192.168.20.70 10.168.110.68
ip nat inside source static 192.168.110.129 10.168.110.69
ip nat inside source static 192.168.110.130 10.168.110.70
ip nat inside source static 192.168.110.133 10.168.110.71
ip nat inside source static 192.168.20.41 10.168.110.72
ip nat inside source static 192.168.110.109 10.168.110.73
ip nat inside source static 192.168.30.24 10.168.110.74
ip nat inside source static 192.168.20.35 10.168.110.75
ip nat inside source static 192.168.20.45 10.168.110.76
ip nat inside source static 192.168.20.30 10.168.110.77
ip nat inside source static 192.168.20.50 10.168.110.78
ip nat inside source static 192.168.20.55 10.168.110.79
ip nat inside source static 192.168.20.65 10.168.110.80
ip nat inside source static 192.168.20.75 10.168.110.81
ip nat inside source static 192.168.30.22 10.168.110.83
ip nat inside source static 192.168.20.40 10.168.110.85
ip nat inside source static 192.168.20.60 10.168.110.86
ip nat inside source static 192.168.110.127 10.168.110.87
ip nat inside source static 192.168.110.128 10.168.110.88
ip nat inside source static 192.168.110.132 10.168.110.89
ip nat inside source static 192.168.110.135 10.168.110.90
ip nat inside source static 192.168.30.23 10.168.110.91
ip nat inside source static 192.168.110.142 10.168.110.111
ip nat inside source static 192.168.110.140 10.168.110.112
ip nat inside source static 192.168.110.143 10.168.110.113
ip nat inside source static 192.168.110.141 10.168.110.114
ip nat inside source static 192.168.110.119 10.168.110.119
ip nat inside source static 192.168.110.121 10.168.110.121
ip route 0.0.0.0 0.0.0.0 10.168.110.65 2
ip route 10.168.105.160 255.255.255.224 192.168.200.1
ip route 10.168.105.162 255.255.255.255 10.168.110.65
ip route 10.168.105.163 255.255.255.255 192.168.200.1
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
ip access-list extended NAT
 permit ip 192.168.110.0 0.0.0.255 any
 permit ip 172.168.110.0 0.0.0.255 any
 permit ip 192.168.10.0 0.0.0.255 any
 permit ip 192.168.20.0 0.0.0.255 any
 permit ip 192.168.30.0 0.0.0.255 any
 permit ip 192.168.40.0 0.0.0.255 any
 permit ip 192.168.255.0 0.0.0.255 any
 permit ip 192.168.254.0 0.0.0.255 any
ip access-list extended TEST
 permit ip 192.168.30.0 0.0.0.255 any
 permit ip 192.168.10.0 0.0.0.255 any
 permit ip 192.168.20.0 0.0.0.255 any
!
no logging trap
ipv6 ioam timestamp
!
!
snmp-server community public RO
!
radius server radius
 address ipv4 192.168.110.120 auth-port 1645 acct-port 1646
 key Cisco123
!
!
!
control-plane
!
!
banner motd ^CC
************************************************************************
*                                                                      *
* This computer system is private property.  If you are not authorized *
* to access this system, you must exit immediately.  All activity on   *
* this system is subject to monitoring, recording, and disclosure.     *
* By logging on, you expressly agree to abide by the Corporate         *
* Information Security Policy and expressly consent to such monitoring,* 
* recording, and disclosure.  Unauthorized use or violation of any     *
* policy may result in disciplinary action up to and including         *
* termination of your employment.                                      *
*                                                                      *
************************************************************************
^C
!
line con 0
line aux 0
line 2
 no activation-character
 no exec
 transport preferred none
 transport output all
 stopbits 1
line 0/0/0 0/0/15
 session-timeout 20 
 exec-timeout 0 0
 logging synchronous
 login authentication console
 no exec
 transport input all
 transport output all
 stopbits 1
line vty 0 4
 privilege level 15
 login authentication VTY
 transport input telnet ssh
line vty 5 15
 privilege level 15
 login authentication VTY
 transport input telnet ssh
!
scheduler allocate 20000 1000
ntp master 1
!         
end

COMM-CUCM#  
```


[+] show ip interface brief

``` c
Interface                  IP-Address      OK? Method Status                Protocol
Embedded-Service-Engine0/0 unassigned      YES NVRAM  administratively down down    
GigabitEthernet0/0         unassigned      YES NVRAM  up                    up      
GigabitEthernet0/0.180     10.168.110.66   YES NVRAM  up                    up      
GigabitEthernet0/0.777     unassigned      YES unset  up                    up      
GigabitEthernet0/1         unassigned      YES NVRAM  up                    up      
GigabitEthernet0/1.700     192.168.110.1   YES NVRAM  up                    up      
GigabitEthernet0/1.701     192.168.255.1   YES NVRAM  up                    up      
GigabitEthernet0/1.702     192.168.254.1   YES NVRAM  up                    up      
GigabitEthernet0/1.703     192.168.10.1    YES NVRAM  up                    up      
GigabitEthernet0/1.704     192.168.20.1    YES NVRAM  up                    up      
GigabitEthernet0/1.705     192.168.30.1    YES NVRAM  up                    up      
GigabitEthernet0/1.710     172.168.110.1   YES NVRAM  up                    up      
GigabitEthernet0/1.800     192.168.40.1    YES NVRAM  up                    up      
GigabitEthernet0/1.810     192.168.50.1    YES NVRAM  up                    up      
GigabitEthernet0/1.820     192.168.60.1    YES NVRAM  up                    up      
GigabitEthernet0/1.830     192.168.70.1    YES NVRAM  up                    up      
GigabitEthernet0/1.840     192.168.80.1    YES NVRAM  up                    up      
GigabitEthernet0/1.850     192.168.90.1    YES NVRAM  up                    up      
GigabitEthernet0/1.860     192.168.100.1   YES NVRAM  up                    up      
Async0/0/0                 unassigned      YES unset  down                  down    
Async0/0/1                 unassigned      YES unset  down                  down    
Async0/0/2                 unassigned      YES unset  down                  down    
Async0/0/3                 unassigned      YES unset  down                  down    
Async0/0/4                 unassigned      YES unset  down                  down    
Async0/0/5                 unassigned      YES unset  down                  down    
Async0/0/6                 unassigned      YES unset  down                  down    
Async0/0/7                 unassigned      YES unset  down                  down    
Async0/0/8                 unassigned      YES unset  down                  down    
Async0/0/9                 unassigned      YES unset  down                  down    
Async0/0/10                unassigned      YES unset  down                  down    
Async0/0/11                unassigned      YES unset  down                  down    
Async0/0/12                unassigned      YES unset  down                  down    
Async0/0/13                unassigned      YES unset  down                  down    
Async0/0/14                unassigned      YES unset  down                  down    
Async0/0/15                unassigned      YES unset  down                  down    
Loopback0                  1.1.1.1         YES NVRAM  up                    up      
Loopback1                  10.225.139.33   YES NVRAM  up                    up      
NVI0                       1.1.1.1         YES unset  up                    up      
Tunnel1                    192.168.200.2   YES NVRAM  up                    down    
```