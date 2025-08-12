
On WindowsTemp

```bash
C:\Users\Administrator>route print
===========================================================================
Interface List
  4...00 50 56 9e b6 96 ......Intel(R) 82574L Gigabit Network Connection #2
  5...00 50 56 9e 4c aa ......Intel(R) 82574L Gigabit Network Connection
  1...........................Software Loopback Interface 1
===========================================================================

IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0      10.224.71.1    10.224.71.152     25
      10.224.71.0    255.255.255.0         On-link     10.224.71.152    281
    10.224.71.152  255.255.255.255         On-link     10.224.71.152    281
    10.224.71.255  255.255.255.255         On-link     10.224.71.152    281
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     10.224.71.152    281
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     10.224.71.152    281
===========================================================================
Persistent Routes:
  Network Address          Netmask  Gateway Address  Metric
       10.228.0.0    255.255.255.0     10.240.0.100       1
===========================================================================

IPv6 Route Table
===========================================================================
Active Routes:
 If Metric Network Destination      Gateway
  1    331 ::1/128                  On-link
  1    331 ff00::/8                 On-link
===========================================================================
Persistent Routes:
  None

C:\Users\Administrator>
```

Let's delete that route

```bash
route delete -p 10.228.0.0
```

And add:

```bash
route -p add 10.228.0.0 mask 255.255.255.0 10.240.0.1
```

On Router 

```bash
router01-3567-natlab#ping 128.6.1.74 source GigabitEthernet1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 128.6.1.74, timeout is 2 seconds:
Packet sent with a source address of 10.224.71.80
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 3/3/4 ms
router01-3567-natlab#ping 172.28.132.114 source GigabitEthernet1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.28.132.114, timeout is 2 seconds:
Packet sent with a source address of 10.224.71.80
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
router01-3567-natlab#


```


```bash
81 permit tcp any host 10.224.71.85 eq 3389
82 permit tcp any host 10.224.71.87 eq 3392
```

```bash
ip nat inside source static tcp 10.240.0.100 3389 10.224.71.87 3392 extendable
```


Capture Buffer>

```bash
monitor capture Gig1 interface GigabitEthernet1 both
```

```bash
monitor capture Gig1 match any
```

```bash
monitor capture Gig1 start
```

```bash
monitor capture Gig1 stop
```

```bash
copy capture:MYCAP sftp://administrator@10.224.71.152/E:/MYCAP.pcap
```

```bash
ip nat inside source static tcp 10.228.0.100 3389 10.224.71.88 3395 extendable
83 permit tcp any host 10.224.71.88 eq 3392
```


## Take a packet Capture on Router

```bash
monitor capture Gig1 interface GigabitEthernet1 both
```

```bash
monitor capture Gig1 match any
```

```bash
monitor capture Gig1 start
```

```bash
monitor capture Gig1 stop
```

```bash
monitor capture Gig1 export tftp://10.240.0.101/Gig1.pcap
monitor capture Gig2 export tftp://10.240.0.101/Gig2.pcap
monitor capture Gig3 export tftp://10.240.0.101/Gig3.pcap
```