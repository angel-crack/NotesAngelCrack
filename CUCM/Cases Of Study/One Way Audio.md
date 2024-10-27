Analyze
Ext 3351 to 915-307-6537


Resolution:

After Analyze the Packet Capture,

You need to consider that a MTP is allocated, and we have two parties ConterraDR-SIP-TRUNK (the call is sent out over here) and the Cisco-CP8841/12.6.1 (calling party number who initiated the call)

MTP will have two legs, every one connected to one of the parties, which means that we will have 4 streams:

[+] Cisco-CP8841/12.6.1 > MTP  (sending audio)
[+] Cisco-CP8841/12.6.1 < MTP  (receiving audio)
[+] MTP > ConterraDR-SIP-TRUNK (sending audio)
[+] MTP < ConterraDR-SIP-TRUNK (receiving audio)

From the call logs we know:

[+] Cisco-CP8841/12.6.1 (192.168.156.64:19896) > (192.168.55.55:24678) MTP (sending audio)
[+] Cisco-CP8841/12.6.1 (192.168.156.64:19896) < (192.168.55.55:24678) MTP (receiving audio)
[+] MTP (192.168.55.55:24674) > (192.168.55.50:13402) ConterraDR-SIP-TRUNK (sending audio)
[+] MTP (192.168.55.55:24674) < (192.168.55.50:13402) ConterraDR-SIP-TRUNK (receiving audio)

From .\20221116 - Packet Captures - Ext 3351 to 915-307-6537\CUCM Nodes - CLI Captures\192.168.55.55\packets.cap

On wireshark > Analyze > Enabled protocols... > check rtp_udp

No under Telephony > RTP > RTP Streams > Look for each of the above 4 streams and you will be able to verify that there is audio on any of those.

It can be representated like:

```c

....I think it gonna fail, i dont even hear a ring!  ----------- I think it gonna fail, i dont even hear a ring!...

192.168.156.64:19896  ---------> 192.168.55.55:24678 +++++++++++ 192.168.55.55:24674 ---------> 192.168.55.50:13402
==================================================== +         + =================================================
==================================================== +   MTP   + =================================================
==================================================== +         + =================================================
192.168.156.64:19896  <--------- 192.168.55.55:24678 +++++++++++ 192.168.55.55:24674 <--------- 192.168.55.50:13402

---------------------  Ringing ------------------------------------------------------- Ringing --------------------

```


This means that even that CUCM is allocating MTP that probably does not need, the audio is just fine.

Up to this point we do not have the call flow perfectly define but for sure the following devices are involved:

Cisco-CP8841/12.6.1 > Switch > CUCM > Trunk > What ever

The main issue is that the customer is not even ringing the phone, which means that the ringing is not being arrived to the CP8841, but CUCM is sending it to the following device.

I have checked the packet capture: .\20221116 - Packet Captures - Ext 3351 to 915-307-6537\11-16-22_kenworthy_switchport_capture.pcapng

And there is no RTP streams on it.



We know that the issue is down stream of CUCM, not on CUCM or even upstream, so, the next action plan is the one that i recommend:

[+] On CUCM CLI: utils network traceroute 192.168.156.64
Once you have the list of the devices between CUCM and the phone, the issue should be on one of those, for sure.

[+] If the commands does not show more devices, ask to the customer what could be in betweent, probably a firewall or a switch/router

[+] At least should a switch where the phone is connected to it, i recommend you to get the show run and show ver command on it and open a collaboration wit the correct team.



Let me know if you have any questions further.

Gustavo,

Summary: The Call Siganling and the port are open correctly

[+] Connections: 192.168.156.64:19896 (Trunk) > 192.168.55.55:24678 (MTP) &&& 192.168.55.50:13402(Calling Party) > 192.168.55.55:24674(MTP)

Snippets of the call below:

[+] Invite from 192.168.156.64 

```c
0055605.002 |10:39:20.389 |AppInfo  |SIPTcp - wait_SdlReadRsp: Incoming SIP TCP message from 192.168.156.64 on port 49525 index 23320 with 1675 bytes:
[86118989,NET]
INVITE sip:819153076537@hqcucmsub.fbfcu.org;user=phone SIP/2.0
Via: SIP/2.0/TCP 192.168.156.64:49525;branch=z9hG4bK7525852f
From: "Mike Pinion" <sip:3351@hqcucmsub.fbfcu.org>;tag=b4a8b968dc7500f15c4722d1-0e4b6875
To: <sip:819153076537@hqcucmsub.fbfcu.org>
Call-ID: b4a8b968-dc75009a-4c6b8922-28547bd1@192.168.156.64
Max-Forwards: 70
Date: Wed, 16 Nov 2022 17:39:17 GMT
CSeq: 101 INVITE
User-Agent: Cisco-CP8841/12.6.1
Contact: <sip:cfed2299-d684-60a2-3b0d-6ee01142d887@192.168.156.64:49525;transport=tcp>;+u.sip!devicename.ccm.cisco.com="SEPB4A8B968DC75"
Expires: 180
Accept: application/sdp
Allow: ACK,BYE,CANCEL,INVITE,NOTIFY,OPTIONS,REFER,REGISTER,UPDATE,SUBSCRIBE,INFO
Remote-Party-ID: "Mike Pinion" <sip:3351@hqcucmsub.fbfcu.org>;party=calling;id-type=subscriber;privacy=off;screen=yes
Allow-Events: kpml,dialog
Recv-Info: conference
Recv-Info: x-cisco-conference
Content-Length: 354
Content-Type: application/sdp
Content-Disposition: session;handling=optional

v=0
o=Cisco-SIPUA 12860 0 IN IP4 192.168.156.64
s=SIP Call
b=AS:4064
t=0 0
m=audio 19896 RTP/AVP 0 8 116 18 101
c=IN IP4 192.168.156.64
b=TIAS:64000
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:116 iLBC/8000
a=fmtp:116 mode=20
a=rtpmap:18 G729/8000
a=fmtp:18 annexb=yes
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-15
a=sendrecv
```


[+] Digit Analysis 3351 (CI=51140856) > 819153076537
```c
20055633.011 |10:39:20.392 |AppInfo  |Digit analysis: match(pi="2", fqcn="3351", cn="3351",plv="5", pss="PT-Internal:Kenworthy-internal:Conterra-DR:Conterra-DR-World:Kenworthy-Local:VMPilotPartition:Kenworthy-LD:Kenworthy-World:System CCX PSTN Dialing:Conterra-HQ:Conterra-HQ-World:Conterra-Local", TodFilteredPss="PT-Internal:Kenworthy-internal:Conterra-DR:Conterra-DR-World:Kenworthy-Local:VMPilotPartition:Kenworthy-LD:Kenworthy-World:System CCX PSTN Dialing:Conterra-HQ:Conterra-HQ-World:Conterra-Local", dd="819153076537",dac="1")
20055633.012 |10:39:20.392 |AppInfo  |Digit analysis: analysis results
20055633.013 |10:39:20.392 |AppInfo  ||PretransformCallingPartyNumber=3351
|CallingPartyNumber=3351
|DialingPartition=Conterra-DR
|DialingPattern=8.1915[2-9]XXXXXX
```


[+] CONTERRA-SIP-RL > ConterraDR-SIP-TRUNK
```c
20055634.003 |10:39:20.393 |AppInfo  |SMDMSharedData::findLocalDevice - Name=CONTERRA-SIP-RL Key=4a6c083d-973f-baec-4d72-b45c142076e4 isActvie=1 Pid=(3,173,15) found

20055651.003 |10:39:20.393 |AppInfo  |SMDMSharedData::findLocalDevice - Name=ConterraDR-SIP-TRUNK Key=0d461be0-f97e-39d9-e25a-a25e828bfeb6 isActvie=1 Pid=(3,181,12) found
20055652.000 |10:39:20.394 |SdlSig   |DmPidRes                               |select_facility                |RouteListCdrc(3,100,172,102714)  |DeviceManager(3,100,53,1)        |3,100,251,32158.6667^192.168.156.64^SEPB4A8B968DC75 |[R:N-H:0,N:0,L:0,V:0,Z:0,D:0]  Cepn=0d461be0-f97e-39d9-e25a-a25e828bfeb6 Id=0 ccmType=8 DeviceName=ConterraDR-SIP-TRUNK Pid=3,100,181,12,ad243d17-98b4-4118-8feb-5ff2e1b781ac
20055652.001 |10:39:20.394 |AppInfo  |RouteListCdrc::select_facility_DmPidRes: Execute a route action.
20055652.002 |10:39:20.394 |AppInfo  |RouteListCdrc::algorithmCategorization -- CDRC_SERIAL_DISTRIBUTION type=1
20055652.003 |10:39:20.394 |AppInfo  |RouteListCdrc::routeAction -- current device name=0d461be0-f97e-39d9-e25a-a25e828bfeb6, idle or available 
20055652.004 |10:39:20.394 |AppInfo  |RouteListCdrc::distributeCall ccSetupReq CI=51140857 BRANCH=0
20055652.005 |10:39:20.394 |AppInfo  |RouteListCdrc::executeRouteAction: EXTEND_CALL_TO_CURRENT_MEMBER -- Success
```


[+] Trunk configured for either BEEO or EO, but this config will not be effective due to other configuration - MTPRequired=1
```c
20055655.009 |10:39:20.394 |AppInfo  |setLocalDtmfCaps: supportedDTMFMethod=3, mWantDtmfReception=1, mPeersWantDtmfReceptionFlag=0, mDtmfPreference=1
20055655.010 |10:39:20.394 |AppInfo  |SIP DTMF Info: mLocalDtmfCaps...UNSOL=1, KPML=1, Inband=1((101:8000,)) mEndppointsDtmfCaps...UNSOL=1, KPML=0, Inband=0(()) mDefaultTelephonyEvent=101, mDtmfPreference=1, mMtpAllocated=0
20055655.011 |10:39:20.394 |AppInfo  |SIPCdpc(319645) - isTrunkConfiguredforVoiceEO: Trunk configured for either BEEO or EO, but this config will not be effective due to other configuration - MTPRequired=1 IPAddrMode=0 MediaPreference=0
20055655.012 |10:39:20.394 |AppInfo  |SIPCdpc(319645) - getEOType: EO Type is set to 0
20055655.013 |10:39:20.394 |AppInfo  |SIPCdpc(319645) - sendPolicyAndCACRegisterReq: capCount[1], videoCap[0], dataCap[0], MMCap[01], earlyOffer[0], latCaps[NULL]
20055655.014 |10:39:20.394 |AppInfo  |SIPCdpc(319645) - sendPolicyAndCACRegisterReq: preconditions: Not using preconditions, rsvpStatus[0] sdp[0]
```


[+] Request to allocate MTP from  MRGL=Conterra-D, MTP_DRCUCM CI=51140858
```c
20055665.000 |10:39:20.395 |SdlSig   |MrmAllocateMtpResourceReq              |waiting                        |MediaResourceManager(3,100,121,1) |SIPCdpc(3,100,180,319645)        |3,100,251,1.1728094^*^*                  |[R:N-H:0,N:0,L:0,V:0,Z:0,D:0]  CI=51140858 MRGLPkid=5aac3a55-2082-4a7f-e46d-7cc66f302127 Kpbs=0 RegionA=Kenworthy CapA=7 RegionB=DRC Lohman CapB=1 SuppressFlag=0 DeviceCapReqd= [0x9 DETECT_2833 PT_2833] MandatoryCapabilities= [0x0] Type=0 Count=1 MTPRequired=T tryPassThru=F
20055665.001 |10:39:20.395 |AppInfo  |MediaResourceManager::waiting_MrmAllocateMtpResourceReq CI=51140858, Count=1

20055672.001 |10:39:20.397 |AppInfo  |MediaResourceCdpc(320633)::resource_rsvp_AllocateMtpResourceRes CI=51140858, DeviceCapability=121, Count=1
20055672.002 |10:39:20.397 |AppInfo  |MediaResourceCdpc(320633)::getMrglGivenDeviceName - DeviceName=MTP_DRCUCM MRGLPkid=5aac3a55-2082-4a7f-e46d-7cc66f302127
```


[+] Open logical channel > 192.168.55.55:24674
```c
20055677.000 |10:39:20.398 |SdlSig-O |MXAgenaOpenLogicalChannel              |NA RemoteSignal                |MediaTerminationPointControl(2,100,122,4) |SIPCdpc(3,100,180,319645)        |3,100,251,1.1728094^*^*                  |[R:N-H:0,N:0,L:0,V:0,Z:0,D:0] codec=4 CI=51140858 mediaType=1 audioRate=20 iLBCMode=1 rfc2833PayloadNum=101 confId=36225990 streamFlag=F partyId=51318103 PT=F PTId=16777216 dyn payloadTypeNum=0 MediaEncrAlgo=0 ConnType=0 ReqIpAddrType=0 ipAddrType=0 ipv4=192.168.2.24:5060 dscpForMyParty=0x0 (DSCP=0x0) dscpForOtherEndParty=0x0 (DSCP=0x0) v150MER=F T38MER=F
20055678.000 |10:39:20.400 |SdlSig-I |MXAgenaOpenLogicalChannelAck           |outCall_waitOLCAck             |SIPCdpc(3,100,180,319645)        |MediaTerminationPointControl(2,100,122,4) |2,100,251,60912.298409^192.168.55.55^MTP_DRCUCM |[R:N-H:0,N:0,L:0,V:0,Z:0,D:0] rc=0 isMultiMedia=F LCN=0 partyId=51318103 port=24674 ipAddrType=0 ipv4=192.168.55.55
```


[+] Once MTP is Allocated, Invite is sent out to 192.168.55.50 ADTRAN_NetVanta_3140/R13.6.0
```c
20055696.001 |10:39:20.400 |AppInfo  |SIPTcp - wait_SdlSPISignal: Outgoing SIP TCP message to 192.168.55.50 on port 5060 index 24203 
[86118991,NET]
INVITE sip:19153076537@192.168.55.50:5060 SIP/2.0
Via: SIP/2.0/TCP 192.168.2.24:5060;branch=z9hG4bK1e08ed49a6f32d
From: "Mike Pinion" <sip:9155621172@192.168.2.24>;tag=27995138~8b591a97-ea6f-4c8b-beb2-e12e8b7660db-51140857
To: <sip:19153076537@192.168.55.50>
Date: Wed, 16 Nov 2022 17:39:20 GMT
Call-ID: 98839980-1ee183d8-100267-1802a8c0@192.168.2.24
Supported: 100rel,timer,resource-priority,replaces
Min-SE:  1800
User-Agent: Cisco-CUCM12.5
Allow: INVITE, OPTIONS, INFO, BYE, CANCEL, ACK, PRACK, UPDATE, REFER, SUBSCRIBE, NOTIFY
CSeq: 101 INVITE
Expires: 180
Allow-Events: presence, kpml
Supported: X-cisco-srtp-fallback,X-cisco-original-called
Call-Info: <sip:192.168.2.24:5060>;method="NOTIFY;Event=telephone-event;Duration=500"
Session-ID: 27b0e2d000105000a000b4a8b968dc75;remote=00000000000000000000000000000000
Cisco-Guid: 2558761344-0000065536-0000308372-0402827456
Session-Expires:  1800
P-Asserted-Identity: "Mike Pinion" <sip:9155621172@192.168.2.24>
Remote-Party-ID: "Mike Pinion" <sip:9155621172@192.168.2.24>;party=calling;screen=yes;privacy=off
Contact: <sip:9155621172@192.168.2.24:5060;transport=tcp>;+u.sip!devicename.ccm.cisco.com="SEPB4A8B968DC75"
Max-Forwards: 69
Content-Type: application/sdp
Content-Length: 205

v=0
o=CiscoSystemsCCM-SIP 27995138 1 IN IP4 192.168.2.24
s=SIP Call
c=IN IP4 192.168.55.55
t=0 0
m=audio 24674 RTP/AVP 0 101
a=rtpmap:0 PCMU/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-15
```


[+] Receiving Session in progress
```c
20055860.002 |10:39:21.816 |AppInfo  |SIPTcp - wait_SdlReadRsp: Incoming SIP TCP message from 192.168.55.50 on port 5060 index 24203 with 874 bytes:
[86118998,NET]
SIP/2.0 183 Session Progress
From: "Mike Pinion"<sip:9155621172@192.168.2.24>;tag=27995138~8b591a97-ea6f-4c8b-beb2-e12e8b7660db-51140857
To: <sip:19153076537@192.168.55.50>;tag=526ebcf0-7f000001-13c4-2b73fb-59b6ec6d-2b73fb
Call-ID: 98839980-1ee183d8-100267-1802a8c0@192.168.2.24
CSeq: 101 INVITE
Via: SIP/2.0/TCP 192.168.2.24:5060;branch=z9hG4bK1e08ed49a6f32d
Contact: <sip:19153076537@192.168.55.50:5060;transport=TCP>
Supported: 100rel,replaces
Allow: ACK, BYE, CANCEL, INFO, INVITE, NOTIFY, OPTIONS, PRACK, REFER, REGISTER
User-Agent: ADTRAN_NetVanta_3140/R13.6.0
Content-Type: application/sdp
Content-Length: 243

v=0
o=BroadWorks 326066361 1 IN IP4 192.168.55.50
s=-
c=IN IP4 192.168.55.50
t=0 0
m=audio 13402 RTP/AVP 0 101
a=sendrecv
a=ptime:20
a=bsoft: 1 image udptl t38
a=rtpmap:0 PCMU/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-15
```



[+] Unrecognize attributes
```c
20055862.014 |10:39:21.816 |AppInfo  |DET-SDPMsg- Unrecognized attributes list: a=bsoft:1 image udptl t38 
```


[+] MediaConnectRequest(51140856,51140857)
```c
20055887.000 |10:39:21.817 |SdlSig   |MediaConnectRequest                    |wait                           |ConnectionManager(3,100,44,1)    |MatrixControl(3,100,117,546223) 
20055887.001 |10:39:21.817 |AppInfo  |ARBTRY-ConnectionManager-wait_MediaConnectRequest(51140856,51140857)
20055887.002 |10:39:21.817 |AppInfo  |ARBTRY-ConnectionManager- storeMediaInfo(CI=51140856): ADD NEW ENTRY, size=49
20055887.003 |10:39:21.817 |AppInfo  |ARBTRY-ConnectionManager- storeMediaInfo(CI=51140857): ADD NEW ENTRY, size=50
20055889.001 |10:39:21.817 |AppInfo  |SIG-MediaManager-(473607)::wait_MediaConnectRequest, CI(51140856,51140857), capCount(7,1), mcNodeId(0,0), xferMode(16,16), reConnectType(0), mrid (0, 51140858) IFCreated(0 0) proIns(0 0), AC(0,0), party1DTMF(1 3 (101:8000,) 0 0) party2DTMF(1 2 (101:8000,) 1 1),reConnFlag=0, connType(3,3), IFHand(0,0),MTP(0,2),MRGL(ab4a1f28-2c63-d3ae-7652-4f2c19cd8fae,5aac3a55-2082-4a7f-e46d-7cc66f302127) videoCap(0 0), mmCallType(0),FS(0,0), IpAddrMode(0 0) aPid(3, 180, 319644), bPid(3, 180, 319645) EOType(0 0) () honorCodec(0 0) ControlProcessType(0 0)
```


[+] Getting the Pre-Allocated MTP on CI=51140858
```c
20055896.000 |10:39:21.817 |SdlSig   |MrmGetPreAllocatedResourceReq          |waiting                        |MediaResourceManager(3,100,121,1) |MediaManager(3,100,119,473607)   |3,100,251,33501.3848^192.168.55.50^*     |[R:N-H:0,N:0,L:0,V:0,Z:0,D:0]  CI=51140858
20055896.001 |10:39:21.817 |AppInfo  |MediaResourceManager::waiting_MrmGetPreAllocatedResourceReq - Found resource in MTPControl list for ci=51140858
20055897.000 |10:39:21.817 |SdlSig   |MrmGetPreAllocatedResourceRes          |waitQueryConnectionResponse    |MediaManager(3,100,119,473607)   |MediaResourceManager(3,100,121,1) |3,100,251,33501.3848^192.168.55.50^*     |[R:N-H:0,N:0,L:0,V:0,Z:0,D:0]  CI=51140858 ConvId=36225990 Caps=6 Name=MTP_DRCUCM Pid=2,100,122,4 Type=0 Region=System Devices deviceCap= [0x79 DETECT_2833 PT_2833 PT_CAP PORT_CAP RTCP_PT_CAP] Count=1MRGL=5aac3a55-2082-4a7f-e46d-7cc66f302127
```


[+] create connection between party1(51140857) and party2(51140858)
```c
20055897.050 |10:39:21.817 |Created  |                                       |                               |MediaExchange(3,100,114,627381)  |MediaManager(3,100,119,473607)   |                                         |NumOfCurrentInstances: 47
20055897.051 |10:39:21.817 |AppInfo  |SIG-MediaManager-(473607)::createConnections, create connection between party1(51140857) and party2(51140858), newConnCount=1 mrid(51140858 51140858), xferMode(16,10), farEnd(XferMode=16, AC=0, T38=0,FS=0, rf=0, mediaReq=0),addr(0,0,0) EOType(0, 0)
```


[+]  create connection between party1(51140856) and party2(51140858)
```c
20055897.055 |10:39:21.817 |Created  |                                       |                               |MediaExchange(3,100,114,627382)  |MediaManager(3,100,119,473607)   |                                         |NumOfCurrentInstances: 48
20055897.056 |10:39:21.817 |AppInfo  |SIG-MediaManager-(473607)::createConnections, create connection between party1(51140856) and party2(51140858), newConnCount=2 mrid(0 0), xferMode(16,10), farEnd(XferMode=16, AC=0, T38=0,FS=0, rf=0, mediaReq=2),addr(0,0,0) EOType(0, 0)
```


[+] MediaConnectRequest(51140857,51140858),For 51140857 > SIPInterface(3,100,186,378833) & for MTP > MTPAgenaInterface(3,100,12,579316)
```c
20055900.000 |10:39:21.817 |SdlSig   |MediaConnectRequest                    |waitMediaConnectRequest        |MediaExchange(3,100,114,627381)  |MediaManager(3,100,119,473607)   |3,100,251,33501.3848^192.168.55.50^*     |[R:N-H:0,N:1,L:0,V:0,Z:0,D:0] Party1: MR=2 CI=51140857 audioCapCount=1 region=DRC Lohman xferMode=16 mrid=51140858 audioId=51318103 MMCap=0x1 sipConfig: BFCPAllowed=F IXAllowed=F activeCap=0 cryptoCapCount=0 flushIns=0 dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F IFPid=(0,0,0,0) dtMedia=F honorCodec=F EOType=0 ControlProcessType=0  DTMF Caps(1,2,(101:8000,),1,T) confID=0 connType=3 connStatus=0 mtpPre=F teleEve=0 IFCreated=F IFHandling=0 FS=0 mcNodeId=0LatentCaps=null dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F Party2: MR=0 CI=51140858 audioCapCount=6 region=System Devices xferMode=10 mrid=51140858 audioId=51318103 MMCap=0x1 sipConfig: BFCPAllowed=F IXAllowed=F activeCap=0 cryptoCapCount=0 flushIns=0 dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F IFPid=(0,0,0,0) dtMedia=F honorCodec=F EOType=0 ControlProcessType=0  DTMF Caps(1,3,(101:8000,),1,F) confID=36225990 connType=3 connStatus=0 mtpPre=F teleEve=0 IFCreated=F IFHandling=0 FS=0 mcNodeId=0LatentCaps=null dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F reConnType=0 videoCall=F AllowedCallType=0x1 mtpChanged=F precLvl=5 resCap=121 party1.mMediaCoordinatorNodeId=0 party2.mMediaCoordinatorNodeId=0 sideBAns= F
20055900.001 |10:39:21.817 |AppInfo  |DET-MediaExchange-(627381)::wait_MediaConnectRequest, party1(CI=51140857, MXIFHandling=0, MediaReq=2,xferMode=16),party2(CI=51140858, MXIFHandling=0,xferMode=10), farEnd(XferMode=16,AC=0,T38=0,FS=0, mediaReq=0), PT(2,2),connType(3,3), farendIpAddrMode=0
20055900.002 |10:39:21.817 |AppInfo  |DET-MediaExchange-(627381)::addPartyAandPartyB, party1:xferMode=16, party2:xferMode=10, ORIGINATOR_MEDIAMANAGER
20055900.003 |10:39:21.817 |AppInfo  |DET-MediaExchange-(627381)::addPartyAandPartyB, audioIdx(1), video(0), partyA(0), earlyOfferE2E=0 earlyOfferType(0:0)
20055900.004 |10:39:21.817 |AppInfo  |DET-MediaExchange-(627381)::setEOOpaqueMediaInfoForInterface, peerEarlyOfferType(0), farEndEarlyOfferType(0), h323EarlyOfferInfo(0)
20055900.005 |10:39:21.817 |AppInfo  |DET-MediaExchange-(627381)::setEOOpaqueMediaInfoForInterface, peerEarlyOfferType(0), farEndEarlyOfferType(0), h323EarlyOfferInfo(0)
20055900.006 |10:39:21.817 |AppInfo  |DET-MediaExchange-(627381)::setRegionAndHonorIncomingCodecPrefForInterface, Party1: myRegion(DRC Lohman), peerRegion(System Devices), farEndRegion(Kenworthy), myHonorIncomingCodecPref(0), peerHonorIncomingCodecPref(0), farEndHonorIncomingCodecPref(0) Party2: myRegion(System Devices), peerRegion(DRC Lohman), farEndRegion(Kenworthy), myHonorIncomingCodecPref(0), peerHonorIncomingCodecPref(0), farEndHonorIncomingCodecPref(0)
20055900.007 |10:39:21.817 |AppInfo  |DET-MediaExchange-(627381)::addPartyAandPartyB, videoCall(0), normalStart(0), HOP RegionBwKbps[ A=64 V = 384 I = 2147483647 ] , E2E RegionBwKbps[ A=8 V = 384 I = 2147483647 ]  , party1VidCapable(0), party2VidCapable(0) 
20055900.008 |10:39:21.817 |AppInfo  |DET-SIPInterface-(0)- CI=51140857 mrId=51140858 PartySide=65 HOP RegionBwKbps[ A=64 V = 384 I = 2147483647 ]  E2E RegionBwKbps[ A=8 V = 384 I = 2147483647 ]  vid=0 AllowedCallType=0x00000001 MCast=0 port=0 IpAddrMode(my=0 peer=0 farEnd=0) XferMode(peer=10 farEnd=16) mediaReq=2 farEndMedReq=0 PassThru(A=2 V=2) Qos(0,1) vidPref(0) isPartyA(0) otherAgentPorts=0  FSInfo=0
20055900.009 |10:39:21.818 |Created  |                                       |                               |SIPInterface(3,100,186,378833)   |MediaExchange(3,100,114,627381)  |                                         |NumOfCurrentInstances: 48
20055900.010 |10:39:21.818 |AppInfo  |DET-MediaExchange-(627381)::addPartyAandPartyB, party1iport(192.168.55.50 5060),party2ipport(0.0.0.0 0)
20055900.011 |10:39:21.818 |AppInfo  |DET-kirra-ipv6 AgenaInterfaceBase-(0)::initializeMemberData  mNegotiatedIPAddrType = 0 
20055900.012 |10:39:21.818 |AppInfo  |DET-AgenaInterface-created by MX(627381)::CI=51140858 allow2833(1) audioPartyId=51318103, mrid=51140858, connType(3 3 3) IPAddrMode(0 0  0) isMidMTP(0) NegotiatedIpAddrType(0) earlyOffer=0 callPrecLvl=5 MyDTMF(3 1) renegEOMed(0) E2E RegionBwKbps[ A=8 V = 384 I = 2147483647 ]  HOP RegionBwKbps[ A=64 V = 384 I = 2147483647 ] 
20055900.013 |10:39:21.818 |Created  |                                       |                               |MTPAgenaInterface(3,100,12,579316) |MediaExchange(3,100,114,627381)  |                                         |NumOfCurrentInstances: 45
20055901.000 |10:39:21.818 |SdlSig   |Start                                  |start                          |MTPAgenaInterface(3,100,12,579316) |MTPAgenaInterface(3,100,12,579316) |3,100,251,33501.3848^192.168.55.50^*     |*TraceFlagOverrode
20055901.001 |10:39:21.818 |LnkState |LinkAdmin - registerLinkStatus - Pid: [3,12,579316], NodeId: 2 regCount: 1
```


[+] MediaConnectRequest(51140856,51140858), for 51140856 > SIPInterface(3,100,186,378834) & for MTP > MTPAgenaInterface(3,100,12,579317)
```c
20055902.000 |10:39:21.818 |SdlSig   |MediaConnectRequest                    |waitMediaConnectRequest        |MediaExchange(3,100,114,627382)  |MediaManager(3,100,119,473607)   |3,100,251,33501.3848^192.168.55.50^*     |[R:N-H:0,N:6,L:0,V:0,Z:0,D:0] Party1: MR=0 CI=51140856 audioCapCount=7 region=Kenworthy xferMode=16 mrid=0 audioId=0 MMCap=0x1 sipConfig: BFCPAllowed=T IXAllowed=T activeCap=0 cryptoCapCount=0 flushIns=0 dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F IFPid=(0,0,0,0) dtMedia=F honorCodec=F EOType=0 ControlProcessType=0  DTMF Caps(1,3,(101:8000,),0,F) confID=0 connType=3 connStatus=0 mtpPre=F teleEve=0 IFCreated=F IFHandling=0 FS=0 mcNodeId=0LatentCaps=null dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F Party2: MR=0 CI=51140858 audioCapCount=6 region=System Devices xferMode=10 mrid=0 audioId=0 MMCap=0x1 sipConfig: BFCPAllowed=F IXAllowed=F activeCap=0 cryptoCapCount=0 flushIns=0 dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F IFPid=(0,0,0,0) dtMedia=F honorCodec=F EOType=0 ControlProcessType=0  DTMF Caps(1,3,(101:8000,),1,F) confID=36225990 connType=3 connStatus=0 mtpPre=F teleEve=0 IFCreated=F IFHandling=0 FS=0 mcNodeId=0LatentCaps=null dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F reConnType=0 videoCall=F AllowedCallType=0x1 mtpChanged=F precLvl=5 resCap=121 party1.mMediaCoordinatorNodeId=0 party2.mMediaCoordinatorNodeId=0 sideBAns= F
20055902.001 |10:39:21.818 |AppInfo  |DET-MediaExchange-(627382)::wait_MediaConnectRequest, party1(CI=51140856, MXIFHandling=0, MediaReq=0,xferMode=16),party2(CI=51140858, MXIFHandling=0,xferMode=10), farEnd(XferMode=16,AC=0,T38=0,FS=0, mediaReq=2), PT(2,2),connType(3,3), farendIpAddrMode=0
20055902.002 |10:39:21.818 |AppInfo  |DET-MediaExchange-(627382)::addPartyAandPartyB, party1:xferMode=16, party2:xferMode=10, ORIGINATOR_MEDIAMANAGER
20055902.003 |10:39:21.818 |AppInfo  |DET-MediaExchange-(627382)::addPartyAandPartyB, audioIdx(1), video(0), partyA(1), earlyOfferE2E=0 earlyOfferType(0:0)
20055902.004 |10:39:21.818 |AppInfo  |DET-MediaExchange-(627382)::setEOOpaqueMediaInfoForInterface, peerEarlyOfferType(0), farEndEarlyOfferType(0), h323EarlyOfferInfo(0)
20055902.005 |10:39:21.818 |AppInfo  |DET-MediaExchange-(627382)::setEOOpaqueMediaInfoForInterface, peerEarlyOfferType(0), farEndEarlyOfferType(0), h323EarlyOfferInfo(0)
20055902.006 |10:39:21.818 |AppInfo  |DET-MediaExchange-(627382)::setRegionAndHonorIncomingCodecPrefForInterface, Party1: myRegion(Kenworthy), peerRegion(System Devices), farEndRegion(DRC Lohman), myHonorIncomingCodecPref(0), peerHonorIncomingCodecPref(0), farEndHonorIncomingCodecPref(0) Party2: myRegion(System Devices), peerRegion(Kenworthy), farEndRegion(DRC Lohman), myHonorIncomingCodecPref(0), peerHonorIncomingCodecPref(0), farEndHonorIncomingCodecPref(0)
20055902.007 |10:39:21.818 |AppInfo  |DET-MediaExchange-(627382)::addPartyAandPartyB, videoCall(0), normalStart(0), HOP RegionBwKbps[ A=64 V = 384 I = 2147483647 ] , E2E RegionBwKbps[ A=8 V = 384 I = 2147483647 ]  , party1VidCapable(0), party2VidCapable(0) 
20055902.008 |10:39:21.818 |AppInfo  |DET-SIPInterface-(0)- CI=51140856 mrId=0 PartySide=65 HOP RegionBwKbps[ A=64 V = 384 I = 2147483647 ]  E2E RegionBwKbps[ A=8 V = 384 I = 2147483647 ]  vid=0 AllowedCallType=0x00000001 MCast=0 port=0 IpAddrMode(my=0 peer=0 farEnd=0) XferMode(peer=10 farEnd=16) mediaReq=0 farEndMedReq=2 PassThru(A=2 V=2) Qos(0,1) vidPref(0) isPartyA(1) otherAgentPorts=0  FSInfo=0
20055902.009 |10:39:21.818 |Created  |                                       |                               |SIPInterface(3,100,186,378834)   |MediaExchange(3,100,114,627382)  |                                         |NumOfCurrentInstances: 49
20055902.010 |10:39:21.818 |AppInfo  |DET-MediaExchange-(627382)::addPartyAandPartyB, party1iport(192.168.156.64 49525),party2ipport(0.0.0.0 0)
20055902.011 |10:39:21.818 |AppInfo  |DET-kirra-ipv6 AgenaInterfaceBase-(0)::initializeMemberData  mNegotiatedIPAddrType = 0 
20055902.012 |10:39:21.818 |AppInfo  |DET-AgenaInterface-created by MX(627382)::CI=51140858 allow2833(1) audioPartyId=0, mrid=0, connType(3 3 3) IPAddrMode(0 0  0) isMidMTP(0) NegotiatedIpAddrType(0) earlyOffer=0 callPrecLvl=5 MyDTMF(3 1) renegEOMed(0) E2E RegionBwKbps[ A=8 V = 384 I = 2147483647 ]  HOP RegionBwKbps[ A=64 V = 384 I = 2147483647 ] 
20055902.013 |10:39:21.818 |Created  |                                       |                               |MTPAgenaInterface(3,100,12,579317) |MediaExchange(3,100,114,627382)  |                                         |NumOfCurrentInstances: 46
20055903.000 |10:39:21.818 |SdlSig   |Start                                  |start                          |MTPAgenaInterface(3,100,12,579317) |MTPAgenaInterface(3,100,12,579317) |3,100,251,33501.3848^192.168.55.50^*     |*TraceFlagOverrode
20055903.001 |10:39:21.818 |LnkState |LinkAdmin - registerLinkStatus - Pid: [3,12,579317], NodeId: 2 regCount: 1
```


[+] SDP for SIP Interface 1 > 51140857 (Call Leg B, the one that it is connected over the trunk, called party number) 
```c
SDP:: mSipSdp().mIsPresent=1, role=2
  numAudiomLines=1 numVideomLines=0
  numAppplicationmLines=0 numBFCPAppmLines=0 numIXAppmLines=0 numT38faxmLines=0
  anatPresent=0
  Bandwidth:: enabledMask=0x00000000 as=0 ct=0 tias=0 rr=0, rs=0, maxprate=0
  SdpSetupType=0, DTLS Fingerprint=, ektInd=0
  audiomLines[0] =
    Media_mLine:: remoteIpAddress=0x3237a8c0 remoteRtpPortNumber=13402 mSDPMode=0 mediaAttr=0x00000000 -  idle=0
     mLineStackIdx=1 telephonyEvent=(101:8000,) silenceSuppressionFlag=0 midID=-1
    SdpSetupType=0, DTLS Fingerprint=, ektInd=0
    Bandwidth:: enabledMask=0x00000000 as=0 ct=0 tias=0 rr=0, rs=0, maxprate=0
    rtp=1,srtp=0,rtcpmux=0,trafficclass= TCL_UNSPECIFIED
    mLine:enabledMask=0x00000000
    mLine:mContent:mContentTypeCount=0
    numUnknownAttrs=1
    caps[0]:: payloadCapability=4 maxFramesPerPacket=20
```

	
[+] SDP for SIP Interface 2 > 51140856 (Call leg A, the one that initiated the call, first invite) 
```c
SDP:: mSipSdp().mIsPresent=1, role=1
  numAudiomLines=1 numVideomLines=0
  numAppplicationmLines=0 numBFCPAppmLines=0 numIXAppmLines=0 numT38faxmLines=0
  anatPresent=0
  Bandwidth:: enabledMask=0x00000001 as=4064 ct=0 tias=0 rr=0, rs=0, maxprate=0
  SdpSetupType=0, DTLS Fingerprint=, ektInd=0
  audiomLines[0] =
    Media_mLine:: remoteIpAddress=0x409ca8c0 remoteRtpPortNumber=19896 mSDPMode=0 mediaAttr=0x00000000 -  idle=0
     mLineStackIdx=1 telephonyEvent=(101:8000,) silenceSuppressionFlag=0 midID=-1
    SdpSetupType=0, DTLS Fingerprint=, ektInd=0
    Bandwidth:: enabledMask=0x00000004 as=0 ct=0 tias=64000 rr=0, rs=0, maxprate=0
    rtp=1,srtp=0,rtcpmux=0,trafficclass= TCL_UNSPECIFIED
    mLine:enabledMask=0x00000000
    mLine:mContent:mContentTypeCount=0
    caps[0]:: payloadCapability=4 maxFramesPerPacket=20
    caps[1]:: payloadCapability=2 maxFramesPerPacket=20
    caps[2]:: payloadCapability=86 maxFramesPerPacket=20
    ILBC Params:: mode=1 framesPerPacket=20
    caps[3]:: payloadCapability=11 maxFramesPerPacket=20
    caps[4]:: payloadCapability=12 maxFramesPerPacket=20
    caps[5]:: payloadCapability=15 maxFramesPerPacket=20
    caps[6]:: payloadCapability=16 maxFramesPerPacket=20
```

	
[+] MTP interfaces ask for their Capabilities
```c
20055906.000 |10:39:21.818 |SdlSig-O |MediaExchangeAgenaAssociateReq         |NA RemoteSignal                |MediaTerminationPointControl(2,100,122,4) |MTPAgenaInterface(3,100,12,579316) |3,100,251,33501.3848^192.168.55.50^*     |[R:N-H:0,N:12,L:0,V:0,Z:0,D:0] confID=36225990 CI=51140858
20055911.000 |10:39:21.818 |SdlSig-O |MediaExchangeAgenaAssociateReq         |NA RemoteSignal                |MediaTerminationPointControl(2,100,122,4) |MTPAgenaInterface(3,100,12,579317) |3,100,251,33501.3848^192.168.55.50^*     |[R:N-H:0,N:10,L:0,V:0,Z:0,D:0] confID=36225990 CI=51140858
```


[+] For both is just the same 192.168.55.55 : (4,80)(2,80)(25,20)(257,0)(259,0)(258,0)
```c
06392132.000 |10:39:21.827 |SdlSig-O |MediaExchangeAgenaUpdateCapabilities   |NA RemoteSignal                |MTPAgenaInterface(3,100,12,579316) |MediaTerminationPointControl(2,100,122,4) |3,100,251,33501.3848^192.168.55.50^*     |[R:N-H:0,N:1,L:0,V:0,Z:0,D:0] AudioCapCount=6(4,80)(2,80)(25,20)(257,0)(259,0)(258,0) CryptoAudioCapCount0 VideoCapCount=0 CryptoVidCount0 AudioCapDir=0 VideoCapDir=0 DataCapDir=0 ipAddrType=0 ipv4=192.168.55.55 Supp.Payload RFC[0 0 0 0 0 ] CustomPictureFormatCount=0 devCap= [0x79 DETECT_2833 PT_2833 PT_CAP PORT_CAP RTCP_PT_CAP] PortInfoList [ confID=0 callRefID=0] v150MER=F T38MER=F
06392133.000 |10:39:21.827 |LnkState |<SdlLinkHandler::enqueue(0xe438e268)>
06392134.000 |10:39:21.827 |SdlSig-O |MediaExchangeAgenaUpdateCapabilities   |NA RemoteSignal                |MTPAgenaInterface(3,100,12,579317) |MediaTerminationPointControl(2,100,122,4) |3,100,251,33501.3848^192.168.55.50^*     |[R:N-H:0,N:0,L:0,V:0,Z:0,D:0] AudioCapCount=6(4,80)(2,80)(25,20)(257,0)(259,0)(258,0) CryptoAudioCapCount0 VideoCapCount=0 CryptoVidCount0 AudioCapDir=0 VideoCapDir=0 DataCapDir=0 ipAddrType=0 ipv4=192.168.55.55 Supp.Payload RFC[0 0 0 0 0 ] CustomPictureFormatCount=0 devCap= [0x79 DETECT_2833 PT_2833 PT_CAP PORT_CAP RTCP_PT_CAP] PortInfoList [ confID=0 callRefID=0] v150MER=F T38MER=F
```


[+] SIP Interface 1 (ConterraDR-SIP-TRUNK) -> 192.168.55.50:13402
```c
20055922.000 |10:39:21.819 |SdlSig   |MediaExchangeAnswer                    |waitInterfacesCapabilities     |MediaExchange(3,100,114,627381)  |SIPInterface(3,100,186,378833)   |3,100,251,33501.3848^192.168.55.50^*     |[R:N-H:0,N:9,L:0,V:0,Z:0,D:0] ] nAudio=1 stackIdx=1 audioCapCount=1 Caps[4(20)] port=13402 IP= ipAddrType=0 ipv4=192.168.55.50 SDPMode=0 mediaAttr=0x0 SP=F RTP=T SRTP=F idle=F QoS=F enabledMask=0 rtcbFbCount=0LatentCaps=null TCL_UNSPECIFIED ptime=20 ~nVideo=0 ~nApp=0 ~nT38Fax=0 ^nBFCPApp=0 ^nIXApp=0 DTMFMethod=2 DTMFConfig=1 RFC2833PT=(101:8000,) wantDTMF=1 provideOOB=T keepAudiomLineForT38=F FaxInviteWithValidIP=F FCOffer=0 transID=0 mANATAddrPref=3Ip_Invalid negIpAddrType=0V4 MTPAllocated=0 sipDevData= SipCallState=1 SIPAccessDevice=T RSVPLastCollab=F cfgAddrMode=0 ReqInactiveSDPMidCall=F BFCPAllowed=F mIXChannelAllowed=F mAllowMultiCodecsInAnswer=F isAnatEnabled=F ignoreWshIfNonSipPeer=F configTrafficClass=Mixed mAllowRTCPBandwidthModifiers=F CiStr=51140857 devCepn=0d461be0-f97e-39d9-e25a-a25e828bfeb6 devName=ConterraDR-SIP-TRUNK IgnorePT=F DevTC =Unspecified CallTC =Unspecified sdpChanged=F FwdTofarEnd=F mAnswerSentForDoToEoCase=F AnsSentForEarlyMedia=F
```


[+] SIP Interface 2 (Calling Party) -> 192.168.156.64:19896
```c
20055930.000 |10:39:21.819 |SdlSig   |MediaExchangeOffer                     |waitInterfacesCapabilities     |MediaExchange(3,100,114,627382)  |SIPInterface(3,100,186,378834)   |3,100,251,33501.3848^192.168.55.50^*     |[R:N-H:0,N:7,L:0,V:0,Z:0,D:0] ] nAudio=1 stackIdx=1 audioCapCount=7 Caps[4(20),2(20),86(20),11(20),12(20),15(20),16(20)] port=19896 IP= ipAddrType=0 ipv4=192.168.156.64 SDPMode=0 mediaAttr=0x0 SP=F RTP=T SRTP=F idle=F QoS=F enabledMask=0 rtcbFbCount=0LatentCaps=null TCL_UNSPECIFIED ptime=0 ~nVideo=0 ~nApp=0 ~nT38Fax=0 ^nBFCPApp=0 ^nIXApp=0 DTMFMethod=3 DTMFConfig=1 RFC2833PT=(101:8000,) wantDTMF=0 provideOOB=F keepAudiomLineForT38=F FaxInviteWithValidIP=F FCOffer=0 transID=0 mANATAddrPref=3Ip_Invalid negIpAddrType=0V4 MTPAllocated=0 sipDevData= SipCallState=1 SIPAccessDevice=F RSVPLastCollab=F cfgAddrMode=0 ReqInactiveSDPMidCall=F BFCPAllowed=T mIXChannelAllowed=T mAllowMultiCodecsInAnswer=F isAnatEnabled=F ignoreWshIfNonSipPeer=F configTrafficClass=Desktop mAllowRTCPBandwidthModifiers=F CiStr=51140856 devCepn=39710e3e-172c-40ec-92c4-f54525e10a10 devName=SEPB4A8B968DC75 TogMediaDir=F DevTC =Unspecified TryReconn=T SOFromFarEndSIP=F IgnorePTAns=F TogXcoder=F EXPECT_CONNECTINFO_UNSET supOffer=F
```


[+] After capabilities for MTP are receiving they need to be compared with the two SIP interfaces

[+] MTPAgenaInterface(3,100,12,579316) with SIP Interface 1 (ConterraDR-SIP-TRUNK), ip=192.168.55.55, port=24674
```c
20055961.013 |10:39:21.821 |AppInfo  |DET-RegionsServer::matchCapabilities-- savedOption=1, PREF_LIST, regionA=System Devices regionB=DRC Lohman latentCaps(A=0, B=0) kbps=64, capACount=6, capBCount=1
20055961.022 |10:39:21.821 |AppInfo  |DET-AgenaInterfaceBase-(579316)::setDeviceOLCAckInfo, lcn=1, ip=192.168.55.55, port=24674

20055977.000 |10:39:21.824 |SdlSig-I |MediaExchangeAgenaStartTalkingAck      |sessionEstablished             |MTPAgenaInterface(3,100,12,579316) |MediaTerminationPointControl(2,100,122,4) |2,100,251,60912.298413^192.168.55.55^MTP_DRCUCM |[R:N-H:0,N:0,L:0,V:0,Z:0,D:0] result=0 mediaType=1 confID=36225990 partyID=0x30f0d57 CI=51140858 ipAddrType=0 ipv4=192.168.55.55 port=24674
```


[+] MTPAgenaInterface(3,100,12,579317) with SIP Interface 2 (Calling Party), ip=192.168.55.55, port=24678
```c
20055963.009 |10:39:21.821 |AppInfo  |DET-RegionsServer::matchCapabilities-- savedOption=1, PREF_LIST, regionA=System Devices regionB=Kenworthy latentCaps(A=0, B=0) kbps=64, capACount=6, capBCo
```
