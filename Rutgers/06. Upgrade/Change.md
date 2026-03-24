Five 9 Related

```bash
show run | sec voice class tenant 59
show run | sec voice class server-group 59
show run | sec voice class uri 59
show run | sec dial-peer voice 500|dial-peer voice 510
show run | sec voice translation-profile incoming-from-Five9|voice translation-profile outgoing-to-Five9
show run | sec voice translation-rule 599|voice translation-rule 60|voice translation-rule 59
show run | sec voice class srtp-crypto 59|voice class tls-profile 59
show run | sec voice class sip-options-keepalive 59
show run | sec ip access-list extended 100|ip access-list extended 101
show run | sec interface Port-channel1.706
```

```c#
cube02-halsey-nwk#show run | sec voice class tenant 59
voice class tenant 59
  timers connect 100
  connection-reuse
  srtp-crypto 59
  session transport tcp tls
  bind control source-interface Port-channel1.731
  bind media source-interface Port-channel1.731
  no pass-thru content custom-sdp
  early-offer forced
  privacy-policy passthru
cube02-halsey-nwk#show run | sec voice class server-group 59
voice class server-group 59
 ipv4 208.69.29.54 port 5061
 ipv4 162.213.153.52 port 5061 preference 1
 description ***Five9 SIP Servers***
cube02-halsey-nwk#show run | sec voice class uri 59
voice class uri 59 sip
 host ipv4:208.69.29.54
 host ipv4:162.213.153.52
cube02-halsey-nwk#show run | sec dial-peer voice 500|dial-peer voice 510
dial-peer voice 500 voip
 description ***Inbound Calls from Five9 Trunk***
 translation-profile incoming incoming-from-Five9
 session protocol sipv2
 incoming uri via 59
 voice-class codec 1
 voice-class sip tenant 59
 dtmf-relay rtp-nte
 srtp
 no vad
dial-peer voice 510 voip
 description ***Outbound Calls to Five9 Trunk***
 translation-profile outgoing outgoing-to-Five9
 destination-pattern 599...........$
 session protocol sipv2
 session server-group 59
 voice-class codec 1
 voice-class sip tenant 59
 voice-class sip options-keepalive profile 59
 dtmf-relay rtp-nte
 srtp
 fax protocol none
 no vad
cube02-halsey-nwk#show run | sec voice translation-profile incoming-from-Five9|voice translation-profile outgoing-to-Five9
voice translation-profile incoming-from-Five9
 translate calling 599
voice translation-profile outgoing-to-Five9
 translate calling 60
 translate called 59
cube02-halsey-nwk#show run | sec voice translation-rule 599|voice translation-rule 60|voice translation-rule 59
voice translation-rule 59
 rule 1 /^5991\(.*\)/ /+1\1/
voice translation-rule 60
 rule 1 /^91\(..........\)/ /+1\1/
 rule 2 /^9\(..........\)/ /+1\1/
 rule 3 /^[1-9].*/ /+&/
 rule 4 /^\+5991\(..........\)/ /+1\1/
 rule 5 /^5991\(..........\)/ /+1\1/
voice translation-rule 599
 rule 1 /^..........$/ /5991&/
 rule 2 /^011.*/ /599&/
 rule 3 /^1..........$/ /599&/
 rule 4 /^\+1\(..........\)/ /5991\1/
cube02-halsey-nwk#show run | sec voice class srtp-crypto 59|voice class tls-profile 59
voice class srtp-crypto 59
 crypto 1 AES_CM_128_HMAC_SHA1_80
 crypto 2 AES_CM_128_HMAC_SHA1_32
cube02-halsey-nwk#show run | sec voice class server-group 59
voice class server-group 59
 ipv4 208.69.29.54 port 5061
 ipv4 162.213.153.52 port 5061 preference 1
 description ***Five9 SIP Servers***
cube02-halsey-nwk#show run | sec voice class sip-options-keepalive 59
voice class sip-options-keepalive 59
 description SIP OPTIONS PING for Five9
 down-interval 60
 up-interval 15
 retry 3
 transport tcp tls
cube02-halsey-nwk#show run | sec ip access-list extended 100|ip access-list extended 101
ip access-list extended 100
 10 permit ip host 208.69.29.54 host 128.6.3.10
 20 permit ip host 208.69.29.43 host 128.6.3.10
 30 permit ip host 208.69.29.44 host 128.6.3.10
 40 permit ip host 208.69.29.45 host 128.6.3.10
 50 permit ip host 208.69.29.46 host 128.6.3.10
 60 permit ip host 162.213.153.52 host 128.6.3.10
 70 permit ip host 162.213.153.41 host 128.6.3.10
 80 permit ip host 162.213.153.42 host 128.6.3.10
 90 permit ip host 162.213.153.43 host 128.6.3.10
 100 permit ip host 162.213.153.44 host 128.6.3.10
 120 deny   ip any any
ip access-list extended 101
 10 permit ip host 128.6.3.10 host 208.69.29.54
 20 permit ip host 128.6.3.10 host 208.69.29.43
 30 permit ip host 128.6.3.10 host 208.69.29.44
 40 permit ip host 128.6.3.10 host 208.69.29.45
 50 permit ip host 128.6.3.10 host 208.69.29.46
 60 permit ip host 128.6.3.10 host 162.213.153.52
 70 permit ip host 128.6.3.10 host 162.213.153.41
 80 permit ip host 128.6.3.10 host 162.213.153.42
 90 permit ip host 128.6.3.10 host 162.213.153.43
 100 permit ip host 128.6.3.10 host 162.213.153.44
 120 deny   ip any any
cube02-halsey-nwk#show run | sec interface Port-channel1.706

```



Steps

### 1 Remove Dial Peers

```bash
conf t
 no dial-peer voice 500 voip
 no dial-peer voice 510 voip
 no dial-peer voice 400 voip
 no dial-peer voice 210 voip
 no dial-peer voice 220 voip
end
show run | sec dial-peer voice 500|dial-peer voice 510
```

## 2 Remove Five9 translation PROFILES and RULES

```bash
conf t
 no voice translation-profile incoming-from-Five9
 no voice translation-profile outgoing-to-Five9
end

conf t
 no voice translation-rule 59
 no voice translation-rule 60
 no voice translation-rule 599
end

```

## 3 Remove SIP keepalive profile (TLS-based)

```bash
conf t
 no voice class sip-options-keepalive 59
end
```

## 4 Remove SRTP crypto + TLS profile

```bash
conf t
 no voice class srtp-crypto 59
 no voice class tls-profile 59
end
```

## 5 Remove Five9 tenant (transport + bindings)

```bash
conf t
 no voice class tenant 59
end
```
## 6 Remove Five9 routing objects

```bash
conf t
 no voice class server-group 59
 no voice class uri 59 sip
end


show run | sec voice class tenant 59
show run | sec voice class server-group 59
show run | sec voice class uri 59
show run | sec voice class srtp-crypto 59|voice class tls-profile 59
```

## 6 Remove Five9-related ACL lines

```bash
conf t
 no ip access-list extended 100
 no ip access-list extended 101
end
```

```bash
show run | sec ip access-list extended 100|ip access-list extended 101
```

## 7 Remove Five9 IP allowlist entries from “trusted list”

```bash
conf t
 voice service voip
  ip address trusted list
   no ipv4 208.69.29.54 255.255.255.255
   no ipv4 208.69.29.43 255.255.255.255
   no ipv4 208.69.29.44 255.255.255.255
   no ipv4 208.69.29.45 255.255.255.255
   no ipv4 208.69.29.46 255.255.255.255
   no ipv4 162.213.153.41 255.255.255.255
   no ipv4 162.213.153.42 255.255.255.255
   no ipv4 162.213.153.43 255.255.255.255
   no ipv4 162.213.153.44 255.255.255.255
   no ipv4 162.213.153.52 255.255.255.255
 end
```

## 5 Remove all remaining TLS/PKI “stuff” (global cleanup)


```bash
conf t
 sip-ua
  no transport tcp tls v1.2
  no crypto signaling default trustpoint cube02-hill-psc
 exit
end

conf t
 no crypto pki trustpoint cube02-hill-psc
end

```


Upgrade:

```bash
https://software.cisco.com/download/home/286324476/type/282046477/release/Bengaluru-17.6.6a

c8000be-universalk9.17.06.06a.SPA.bin

conf t
no boot system
boot system flash:c8000be-universalk9.17.06.06a.SPA.bin
exit
wr






cube02-halsey-nwk#dir flash: | i .bin
21      -rw-        805646500  May 13 2024 15:35:23 -04:00  c8000be-universalk9.17.06.06a.SPA.bin
17      -rw-        805709807  Jun 13 2023 14:02:41 -04:00  c8000be-universalk9.17.06.05.SPA.bin
11      -rw-        700738067   Jun 4 2023 00:04:39 -04:00  c8000be-universalk9_npe.17.03.04a.SPA.bin
cube02-halsey-nwk#show running-config | include boot system
boot system flash bootflash:/c8000be-universalk9.17.06.05.SPA.bin
cube02-halsey-nwk#show startup-config | include boot system
boot system flash bootflash:/c8000be-universalk9.17.06.05.SPA.bin
cube02-halsey-nwk#

```


```bash
conf t
no ip http secure-server
end
```

```bash
conf t
no crypto pki trustpoint CA-SECTIGO-ROOT
no crypto pki trustpoint Intermidiate1-InCommon
no crypto pki trustpoint UserTrust
no crypto pki trustpoint SLA-TrustPoint
no crypto pki trustpoint cube02-hill-psc
end
```

```bash
conf t
no crypto pki certificate chain CA-SECTIGO-ROOT
no crypto pki certificate chain Intermidiate1-InCommon
no crypto pki certificate chain UserTrust
no crypto pki certificate chain SLA-TrustPoint
no crypto pki certificate chain cube02-hill-psc
end

```