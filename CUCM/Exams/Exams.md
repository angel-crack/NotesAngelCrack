	[+] If a sip trunk has MTP required will a video call work ?
	
	a. Yes, The video always work with an MTP 
	b. Yes, if the MTP is passtrough 
	c. No, Video calls do not work with any MTP 
	d. No, when using MTP Required codec passtrough is not suported.

**Answer ⇒  c. No, when using MTP Required codec passtrough is not suported.**


Enabling this option on the SIP trunk assigns an MTP for every outbound call. This option does not support codec pass-through mode, which imposes a single codec (G.711 or G.729) limitation over the SIP trunk, thus limiting media to voice calls only. **⇒ [Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/srnd/collab11/collab11/media.html#pgfId-1158832)**

	[+] What protocol is used to secure SIP signaling across the CUCM?
	
	a. AES128 
	b. TLS 
	c. HTTPS 
	d. SSL

**Answer ⇒ b. TLS** 

SIP trunks use TLS for signaling encryption. TLS is configured on the SIP Security Profile associated with the SIP trunk, and it uses X.509 certificate exchanges to authenticate trunk devices and to enable signaling encryption. **⇒ [Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/srnd/collab11/collab11/trunks.html#pgfId-1352100)**

	[+] When both devices have a codec less than or equal to Max Audio Bit Rate but neither have compatible codecs, which side will invoke a transcoder?  Select one: 
	
	a. Both sides 
	b. Higher BW side 
	c. Lower BW side 
	d. No transcoder will be allocated

**Answer ⇒ b. Higher BW side**

![[Pasted image 20240404165713.png]]

Unified CM uses media resource groups (MRGs) to enable sharing of MTP and transcoding resources among the Unified CM servers within a cluster. In addition, when using an LBR codec (for example, G.729a) for calls that traverse different regions, the transcoding resources are used only if one (or both) of the endpoints is unable to use the LBR codec.

Unified CM knows that a transcoder is required and allocates one based on the MRGL and/or MRG of the device that is using the higher-bandwidth codec. In this case it is the VM/UM server that determines which transcoder device is used. This behavior of Unified CM is based on the assumption that the transcoder resources are actually located close to the higher-bandwidth device. If this system was designed so that the transcoder for the VM/UM server was located at the remote site, then G.711 would be sent across the WAN, which would defeat the intended design. As a result, if there are multiple sites with G.711-only devices, then each of these sites would need transcoder resources when an LBR is run on the WAN. **⇒ [Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/srnd/collab11/collab11/media.html#marker-1247719)**

	[+] Call flow: ISP_1 >> CUBE_1 >> CUCM (CFA) >> CUBE_2 > > ISP_2 
	Assuming CUBE_1 is sending EO and CUCM has been configured for CUBE_2' SIP Trunk "Early Offer Mandatory". Does CUCM allocate a MTP? Select one: 
	
	a. Yes, and it will share MTP capabilities in the ACK 
	b. No, CUCM will share CUBE_1's capabilities 
	c. Yes, and it will share MTP capabilities in the intial INVITE 
	d. No, CUCM doesn't need an MTP for any Early Offer connection

**Answer ⇒ b. No, CUCM will share CUBE_1's capabilities**

Unified CM does not need to insert an MTP to create an outbound Early Offer call over a SIP trunk if the inbound call to Unified CM is received by any of the following means:

- On a SIP trunk using Early Offer **⇐ This One**

![[Pasted image 20240404171000.png]]

* On an H.323 trunk using Fast Start
- On an MGCP trunk
- From a SIP-based IP phone registered to Unified CM
- From newer SCCP-based Cisco Unified IP Phone models registered to Unified CM
**⇒ [Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/srnd/collab11/collab11/trunks.html#pgfId-1419431)**

	[+] A collaboration engineer is troubleshooting an MOH problem on a Cisco IOS SIP gateway. While searching through a debug ccsip message output, which three parameters in the SIP messages can be used to determine if the call was placed on hold? (Choose three) Select one or more: 
	
	a. SDP with a=SENDONLY 
	b. SDP with c=IN IP4 0.0.0.0 
	c. OPTIONS with 301 CALLHOLD 
	d. SDP with a=INACTIVE 
	e. SDP with c=INACTIVE 
	f. BYE with A = CALLHOLD

**Answer ⇒ a. SDP with a=SENDONLY, b. SDP with c=IN IP4 0.0.0.0, d. SDP with a=INACTIVE**

	[+] CTI RP_CCXA and CTIRP_CCXB are part of the device pool subA-subB where the primary node is SUB A, CCM service on SUB A crashed and the CTI RPs registered with SUB B, Once the CCM service was restored on SUB A, only CTIRP_CCXB registered back with this node. What would be a reason why CTI RP_CCXA did not register back with node SUB A? Select one: 
	
	a. CTI RP_CCXA behaves as expected since it will get registered back only if it is restarted. 
	b. CTIRP_CCXB was incorrectly configured with another devicepool that has SUB B as primary server. 
	c. Database replication issue. 
	d. CTI RP_CCXA will register back when calls are no longer being processed or active.

**Answer ⇒ CTI RP_CCXA will register back when calls are no longer being processed or active.**

![[CTI Redundancy.png]]

**⇒ [Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/jtapi_dev/14_0_1/cucm_b_cisco-unified-jtapi-developers-guide-14/cucm_b_cisco-unified-jtapi-developers-guide-1251_chapter_010.html#ariaid-title162)**

	[+] Cisco SIP IP telephony is implemented on two floors of your company. Afterward, users report intermittent voice issues in calls established between floors. All calls are established, and sometimes they work well, but sometimes there is one way audio or no audio. You determine there is a firewall between the floors, and the admin reports that it is allowing SIP signaling and UDP ports from 20000 to 22000 bidirectionally. What are two possible solutions? (Choose two):
	
	a. Go to the System Parameters in CUCM and change the range of media ports to 20000 - 22000 
	b. Ask the firewall administrator to change the range of UDP ports to 16384-32767 
	c. Go to the SIP profile assigned to these IP phones in CUCM and change the range of media ports to 163844 - 32767
	d. Ask the firewall administrator to change the ports to TCP 
	e. Go to the SIP profile assigned to these IP phones in CUCM and change the range of media ports to 20000 - 22000


**Answer ⇒** 

**b. Ask the firewall administrator to change the range of UDP ports to 16384-32767**

**e. Go to the SIP profile assigned to these IP phones in CUCM and change the range of media ports to 20000 - 22000**


**⇒ [Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/admin/11_5_1/sysConfig/11_5_1_SU1/cucm_b_system-configuration-guide-1151su1/cucm_b_system-configuration-guide-1151su1_chapter_01010101.html#CUCM_RF_TD204392_00__table300)**



	[+] Customer complains that they are receiving delayed offer INVITE in mid-call when inserting MOH, Reviewing the SIP trunk configuration they have the Early offer enable and checking the traces it has been confirmed that the first INVITE was with Early Offer.
	
	![[Pasted image 20240404191347.png]]
	
	Is the behavior explained above expected?

	[+] ABC company has decided to implement hunt groups to help distribute calls between members. in order to implement this, the administrator must configure hunt list, hunt group, and line groups on CUCM. Which distribution algorithms should the administrator implement:
	
	a. sequential, circular, broadcast 
	b. Top down, circular, broadcast 
	c. top down, round robin, broadcast 
	d. Top down, round robin, distribute

**Answer ⇒ b. Top Down, circular, boradcast.**

On the line group configuration we can visualize what options are available.

![[Pasted image 20240404193457.png]]

	[+] A user reports that when they call a specific phone number, no one answers the call, but when they call from a mobile phone, the call is answered. The engineer troubleshooting the issue is expecting the far-end gateway to cut through audio on the 183 Session in Progress SIP message. Which SIP profile configuration element is necessary for CUCM to send acknowledgment of provisional responses? 
	
	a. The configuration parameter SIP Rel1XX Options must be set to Send PRACK for all 1xx Messages 
	b. Accept Audio Codec Preferences in Received Offer must be set to ON 
	c. Allow Passthrough of Configured Line Device Caller Information must be enabled. 
	d. Early Offer for G Clear calls must be enabled

**Answer ⇒ a. The configuration parameter SIP Rel1XX Options must be set to Send PRACK for all 1xx Messages**

By default, CUCM does not support reliable response. However, you can change the SIP trunk profile in order to configure it:

1. In the CUCM Administration interface, go to **Device > Device Setting > SIP Profile**.
2. Open the SIP profile used by a given SIP trunk.
3. Choose **Send PRACK for all 1xx Messages** from the SIP Rel1XX Options drop-down list.
4. Reset the SIP trunk profile for the given SIP trunk.

![[Pasted image 20240404195605.png]]

[Documentation](https://www.cisco.com/c/en/us/support/docs/voice/session-initiation-protocol-sip/116086-configure-cube-cucm-sip-00.html#toc-hId--782471649)

	[+] Which SDP content headers can be found in a SIP INVITE message? (Choose two.) Select one or more: 
	
	a. Connection Info 
	b. Expires 
	c. Allow 
	d. Media Attributes 
	e. CSeq 
	f. Contact

**Answer ⇒** 
**a. Connection Info**
**d. Media Attributes**

![[Pasted image 20240404201114.png]]

**⇒ [Documentation](https://datatracker.ietf.org/doc/html/rfc4566#page-9)**

	[+] Which description of the Standard Local Route Group deployment is true? 
	
	a. Chooses the route group that is configured under the device pool of the called-party device. 
	b. Can be assigned directly to the route pattern 
	c. Can be associated under the route group 
	d. Chooses the route group that is configured under the device pool of the calling-party device.

The system automatically replaces this route group with the name of the local route group that is provisioned for the calling device.

**⇒ [Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/admin/10_0_1/ccmfeat/CUCM_BK_F3AC1C0F_00_cucm-features-services-guide-100/CUCM_BK_F3AC1C0F_00_cucm-features-services-guide-100_chapter_0100110.html#ariaid-title5)**

	[+] Match the following messages to the protocol they belong to:
	
	NTFY ⇒ MGCP
	CRCX ⇒ MGCP
	SETUP ⇒ H323 
	Alerting ⇒ H323
	Refer ⇒ SIP
	302 Moved Temporarily ⇒ SIP

H225 Signaling for H323
![[Pasted image 20240404202119.png]]

MGCP Signaling
![[Pasted image 20240404202200.png]]

	[+] Call flow: ISP_1 >> CUBE_1 >> CUCM (CFA) >> CUBE_2 > > ISP_2 Assuming on SIP trunk to CUBE_2, CUCM has been configured "MTP Required" and Early Offer "Disabled". Which of following statements is true regarding CUCM - CUBE_2 connection? Select one: 
	
	a. CUCM will send a EO INVITE and MTP capabilities will be shared in the ACK 
	b. CUCM will send a EO INVITE to CUBE_2 sharing MTP capabilities 
	c. CUCM will send a EO INVITE sharing CUBE_1 capabilities 
	d. CUCM will send a DO INVITE and MTP capabilities will be shared in the ACK 
	e. CUCM will send a DO INVITE to CUBE_2 sharing MTP capabilities

**Answer ⇒ b. CUCM will send a EO INVITE to CUBE_2 sharing MTP capabilities** 

The Trunk is set as:

![[Pasted image 20240404202740.png]]

**Media Termination Point Required**

Enabling this option on the SIP trunk assigns an MTP for every outbound call. This option does not support codec pass-through mode, which imposes a single codec (G.711 or G.729) limitation over the SIP trunk, thus limiting media to voice calls only. With this option enabled, calls over the trunk uses MTPs assigned to the trunk rather than using calling device MTPs, which forces the media to follow the same signaling path.

**Note**![](https://www.cisco.com/c/dam/en/us/td/i/templates/blank.gif) Enabling the **Media Termination Point Required** option on the SIP trunk increases MTP usage because an MTP is assigned for every inbound and outbound call rather than on an as-needed basis.

**⇒ [Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/srnd/collab11/collab11/media.html#pgfId-1158832)**

	[+] Phone A is calling to a long distance number however user complains that no ring back is being played but the call connects successfully. As TAC engineer you confirmed that the signaling is good and the phone is receiving 183 Session Progress with SDP instead of 180 Ringing. What would be a possible reason of this behavior? Select one: 
	
	a. It is a phone issue since the phone plays the ring back locally 
	b. The called device is not sending any media before the 200 OK 
	c. CUCM is not able to allocate an ANN to play the ringback 
	d. the IP phone couldn’t download the ringback file properly

**Answer ⇒ c. CUCM is not able to allocate an ANN to play the ringback** 

An annunciator is a software function of the Cisco IP Voice Media Streaming Application that provides the ability to stream spoken messages or various call progress tones from the system to a user.

- Integration via SIP trunk

SIP endpoints have the ability to generate and send tones in-band in the RTP stream. Because SCCP devices do not have this ability, an annunciator is used in conjunction with an MTP to generate or accept DTMF tones when integrating with a SIP endpoint. The following types of tones are supported:

–![](https://www.cisco.com/c/dam/en/us/td/i/templates/blank.gif) Call progress tones (busy, **alerting**, reorder, and ringback)

[Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/srnd/collab11/collab11/media.html#pgfId-1046552)

	[+] What is IMDB database and at what moment is it created? Select one: 
	
	a. IMDB is a UCM Informix database stored in the HDD. It is created during the CCM service start up process. 
	b. IMDB is a UCM database stored in the RAM which contains mirrored information from IDS such as Numbering plan and device tables. It is created during the CCM service start up process. 
	c. IMDB is a UCM Informix database stored in the HDD. It is created during the CUCM installation. 
	d. IMDB is a UCM database stored in the RAM which contains mirrored information from IDS such as Numberingplan and device tables. It is created during the CUCM installation.

**Answer ⇒ b. IMDB is a UCM database stored in the RAM which contains mirrored information from IDS such as Numbering plan and device tables. It is created during the CCM service start up process.**

[Documentation](https://community.cisco.com/t5/ip-telephony-and-phones/cucm-db-explaination-what-is-db-ids-and-imdb-db-functionality/td-p/2881547)

	[+] Taken into consideration the parameter “Early Offer support for voice and video calls Mandatory (insert MTP if needed)”, when is a MTP going to be allocated? Select one: 
	
	a. MTP will be invoked if the called device cannot provide Unified CM with the media characteristics required to create the inbound Early Offer. 
	b. MTP will be invoked when having a DTMF mismatch. 
	c. MTP will be invoked if the calling device cannot provide Unified CM with the media characteristics required to create the inbound Early Offer. 
	d. MTP will be invoked every time CUCM tries to extend a call. 
	e. MTP will be invoked if the Called device cannot provide Unified CM with the media characteristics required to create the outbound Early Offer. 
	f. MTP will be invoked if the calling device cannot provide Unified CM with the media characteristics required to create the outbound Early Offer.

**Answer ⇒ f. MTP will be invoked if the calling device cannot provide Unified CM with the media characteristics required to create the outbound Early Offer.**

For outbound calls over a SIP trunk configured as Early Offer support for voice and video calls Mandatory (insert MTP if needed), Unified CM inserts an MTP to create an SDP Offer in the following cases only:

- An inbound call to Unified CM is received over a Delayed Offer SIP trunk
- An inbound call to Unified CM is received over an H.323 Slow Start trunk
- An inbound call is received from an older SCCP-based IP phone registered to Unified CM

[Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/srnd/collab11/collab11/trunks.html#pgfId-1343981)

	[+] A customer calls in to report an issue with CER history. He stated that he was working from home and having issues with his wife due to the amount of time that he has invested on trying to find a reason why he cannot see emergency calls on the CER history option. While checking the 911 CTI RP configuration, he realizes that it is unregistered.  The customer has limited access because he is working remotely, so he was only able to pull JTAPI logs from the system. He is demanding an explanation on why this issue is happening so he can stop working on this and he can make up with his wife.  Your job is to find the reason why this issue is happening based on the following output:

9364: Nov 23 07:09:02.973 COT %JTAPI-PROTOCOL-7-UNK:(P53-192.168.10.82) [ProviderRetryThread] sending: com.cisco.cti.protocol.ProviderOpenRequest {
provider = 192.168.10.82
qbeClientVersion = Cisco JTAPI 12.5(0.98000)-1 Release
login = com.cisco.cti.protocol.UnicodeString {
unicodedisplayName = MyCER
count = 5
sizeofElement = 2
}
filter = com.cisco.cti.protocol.ProviderEventFilter {
deviceRegistered = true
deviceUnregistered = true
directoryChangeNotify = true
deviceConfigChangeNotify = true
deviceEMLoginStatusChangeNotify = true
deviceMultiMediaCapabilityChangedNotify = true
}
applicationID = JTAPI[1]@cer125
desiredServerHeartbeatTime = 30
requestTimer = 0
cmAssignedApplicationID = 0
pluginName = CiscoJTAPI
applicationPriority = 2
lightWeightOpen = false
authenticationType = 0
requestOldDeviceLineFetch = false

9365: Nov 23 07:09:02.974 COT %JTAPI-PROTOCOL-7-UNK:(P53-192.168.10.82) received Response: com.cisco.cti.protocol.ProviderOpenResponse {
sequenceNumber = 17
result = 0
providerInfoString = 12.5.1.11900-146.i386
clientHeartbeat = 30
serverHeartbeat = 30
requestTimer = 5
pluginVersion = 12.5.1.11900-3.i386
pluginLocation = http://192.168.10.82/plugins/
providerId = 16778994
fipsCompliant = false
}

9366: Nov 23 07:09:02.974 COT %JTAPI-CTIIMPL-7-UNK:(P53-192.168.10.82) Server response: will send server heartbeat every 30 seconds

9367: Nov 23 07:09:02.975 COT %JTAPI-CTIIMPL-7-UNK:(P53-192.168.10.82) Server response: expecting client heartbeat every 30 seconds

9372: Nov 23 07:09:03.121 COT %JTAPI-PROTOCOL-7-UNK:(P53-192.168.10.82) received Event: com.cisco.cti.protocol.ProviderOpenCompletedEvent {
eventSequence = 211
reason = -1932787425
sequenceNumber = 17
providerInfoString = 12.5.1.11900-146.i386
clientHeartbeat = 0
serverHeartbeat = 0
**failureDescription = Directory login failed - locked due to too many attempts.**
bMonitorCallParkDNs = false
providerId = 16778994
precedenceValue = 0
canSupportIPv6 = false
loginUserName = com.cisco.cti.protocol.UnicodeString {
unicodedisplayName =
count = 0
sizeofElement = 2
}
**daysUntilPasswordExpiry = -1**
totalControllableDevices = 0
clusterID =
}
9374: Nov 23 07:09:03.122 COT %JTAPI-PROTOCOL-7-UNK:(P53-192.168.10.82) received Event: com.cisco.cti.protocol.ProviderClosedEvent {
eventSequence = 212
reason = 4
}
9375: Nov 23 07:09:03.122 COT %JTAPI-MISC-7-UNK:(P53-192.168.10.82) EventThread: queuing com.cisco.cti.protocol.ProviderClosedEvent

9376: Nov 23 07:09:03.122 COT %JTAPI-CTIIMPL-7-UNK:(P53-192.168.10.82) EventThread handling event com.cisco.cti.protocol.ProviderOpenCompletedEvent[211]

9377: Nov 23 07:09:03.122 COT %JTAPI-PROTOCOL-7-UNK:QoS parameter Set To0

9378: Nov 23 07:09:03.122 COT %JTAPI-CTIIMPL-7-UNK:(P53-192.168.10.82) EventThread handling event com.cisco.cti.protocol.ProviderClosedEvent[212]

9379: Nov 23 07:09:03.122 COT %JTAPI-PROTOCOL-7-UNK:(P53-192.168.10.82) received Event: com.cisco.cti.protocol.ProviderOutOfServiceEvent {
eventSequence = 213
PROVIDER_OUT_OF_SERVICE_EVENT = 200
}

Select one: 
a. CTI manager service is not properly responding and a service restart should be make. 
b. We cannot determine the root cause for this issue based on the information provided.  
c. This customer is experiencing a network connectivity issue and the CER server is unable to pull the call information to update the CER history entries. 
d. CER application user was incorrect or credentials were not properly provided.

**Answer ⇒  d. CER application user was incorrect or credentials were not properly provided.**

Password is expired.


	[+] The customer experienced the following problem: 
	- Delay making calls, some outbound call were not working and they receive a message “Can't Complete the Call, contact your System Administrator.” 
	- After that the phones went unregistered 
	- Issue hasn't re-occurred, customer wants an RCA
	Two alerts were triggered:
	
	%CCM_CALLMANAGER-CALLMANAGER-3-CodeYellowEntry: CodeYellowEntry Expected Average Delay:7906 Entry Latency:20 Exit Latency:8 Sample Size:10 Total Code Yellow Entry:4 High Priority Queue Depth:0 Normal Priority Queue Depth:5161 Low Priority Queue Depth:0 App ID:Cisco CallManager Cluster
	
	 %UC_RTMT-2-RTMT_ALERT: % AlertName=CoreDumpFileFound AlertDetail= At Mon Aug 21 22:22:27 EST 2017 on node 172.16.136.21, the following CoreDumpFileFound events generated: # 012TotalCoresFound : 1#012CoreDetails : The following lists up to 6 cores dumped by corresponding applications.#012Core1 : Cisco CallManager (core.11596.6.ccm.1503372096)
	
	What  information will you request to further troubleshoot this? Select one: 
	a. Perfmon Logs, backtrace of the core dump and Call manager traces 
	b. Perfmon logs,backtrace of the core dump,Event viewer system/application logs and  show commands like show process using-most cpu 
	c. Perfmon logs,backtrace of the core dump,call manager traces, event viewer system/application logs 
	d. Perfmon logs and Event viewer system/application logs

**Answer ⇒ b. Perfmon logs,backtrace of the core dump,Event viewer system/application logs and  show commands like show process using-most cpu**

	[+] When using SNR What voicemail policy prevents the call to go to the cellphone's voicemail based on the user interaction? Select one: 
	
	a. User facing control  
	b. That is not possible, SNR always uses a Timer 
	c. Timer control 
	d. DTMF control  
	e. User control

**Answer ⇒ e. User Control**

When you configure SNR, you will have some timers availables.

One of them is the policy for going to voicemail.

Timer Control ⇒ The VM will have a delay to go to VM
User Control ⇒ Prompt to the User will be ask before sent to VM.

[Documentation](https://www.cisco.com/c/en/us/support/docs/unified-communications/unified-communications-manager-callmanager/200447-Single-Number-Reach-Feature-for-Cisco-Un.html#toc-hId-1182592290)

	[+] How many CTI manager servers can run on a cluster? Select one: 
	a. 4 
	b. 2 
	c. 3 
	d. 8

**Answer ⇒ d.8**

![[Pasted image 20240404222932.png]]

Cisco Call Manager Service must be running on the node where we want to run CTI Manager, because standalone CTI  is not supported. Thus, if we can have 21 nodes in a cluster where 4 actives CCM Service Nodes plus 4 on standy, we can have 8 CTI Servers

[Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/srnd/collab12/collab12/callpros.html#pgfId-1083021)

	[+] A CTI application can connect to two CTI Managers (CUCM nodes) simultaneously. 
	Select one: 
	a. True 
	b. False

**Answer ⇒ True**


For CTI applications that require redundancy, the TAPI TSP or JTAPI client can be configured with two IP addresses, thereby allowing an alternate CTI Manager to be used in the event of a failure. It should be noted that this redundancy is not stateful in that no information is shared and/or made available between the two CTI Managers, and therefore the CTI application will have some degree of re-initialization to go through, depending on the exact nature of the failover.

When a CTI Manager fails-over, just the CTI application login process is repeated on the now-active CTI Manager. Whereas, if the Unified CM server node itself fails, then the re-initialization process is longer due to the re-registration of all the devices from the failed Unified CM to the now-active Unified CM, followed by the CTI application login process.

For CTI applications that require load balancing or that could benefit from this configuration, the CTI application can simply connect to two CTI Managers simultaneously. **⇒ [Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/srnd/collab12/collab12/callpros.html#pgfId-1084418)**



	[+] As we know from documentation, once a SIP phone registers to the CUCM, keep-alive are sent every 120 seconds as per the settings in CUCM.
	Are these the correct expire values for a keepalive between the phone and its primary ccm node?
	
REGISTER sip:192.168.20.172 SIP/2.0
Via: SIP/2.0/TCP 192.168.110.240:50057;branch=z9hG4bK0259a19a
From: <sip:1003@192.168.20.172>;tag=9c57ad12a4e0004563e5c512-44f61251
To: <sip:1003@192.168.20.172>
Call-ID: 9c57ad12-a4e00002-7f50d2cf-489bc75f@192.168.110.240
Max-Forwards: 70
Session-ID: 1c10cf0000105000a0009c57ad12a4e0;remote=00000000000000000000000000000000
Date: Fri, 29 Mar 2019 00:31:26 GMT
CSeq: 155 REGISTER
User-Agent: Cisco-CP8851/12.1.1
Contact: <sip:f0ae9f65-9fa6-4a32-84ba-4d40048f79c8@192.168.110.240:50057;transport=tcp>;+sip.instance="<urn:uuid:00000000-0000-0000-0000-9c57ad12a4e0>";+u.sip!devicename.ccm.cisco.com="SEP9C57AD12A4E0";+u.sip!model.ccm.cisco.com="684"
Supported: replaces,join,sdp-anat,norefersub,resource-priority,extended-refer,X-cisco-callinfo,X-cisco-serviceuri,X-cisco-escapecodes,X-cisco-service-control,X-cisco-srtp-fallback,X-cisco-monrec,X-cisco-config,X-cisco-sis-7.0.0,X-cisco-xsi-8.5.1
Content-Length: 0
**Expires: 3600**

SIP/2.0 100 Trying
Via: SIP/2.0/TCP 192.168.110.240:50057;branch=z9hG4bK0259a19a
From: <sip:1003@192.168.20.172>;tag=9c57ad12a4e0004563e5c512-44f61251
To: <sip:1003@192.168.20.172>
Date: Fri, 29 Mar 2019 00:31:27 GMT
Call-ID: 9c57ad12-a4e00002-7f50d2cf-489bc75f@192.168.110.240
CSeq: 155 REGISTER
Content-Length: 0

SIP/2.0 200 OK
Via: SIP/2.0/TCP 192.168.110.240:50057;branch=z9hG4bK0259a19a
From: <sip:1003@192.168.20.172>;tag=9c57ad12a4e0004563e5c512-44f61251
To: <sip:1003@192.168.20.172>;tag=1425735795
Date: Fri, 29 Mar 2019 00:31:27 GMT
Call-ID: 9c57ad12-a4e00002-7f50d2cf-489bc75f@192.168.110.240
Server: Cisco-CUCM11.5
CSeq: 155 REGISTER
**Expires: 0**
Contact: <sip:f0ae9f65-9fa6-4a32-84ba-4d40048f79c8@192.168.110.240:50057;transport=tcp>;+sip.instance="<urn:uuid:00000000-0000-0000-0000-9c57ad12a4e0>";+u.sip!devicename.ccm.cisco.com="SEP9C57AD12A4E0";+u.sip!model.ccm.cisco.com="684"
Supported: X-cisco-srtp-fallback,X-cisco-sis-8.0.0
Content-Length: 0


	Select one: 
	True 
	False

Answer ⇒ False

SIP Mechanism for Keep-Alive stands:

* Register to primary with expires 3600, which is the value set on the SIP profile:

![[Pasted image 20240404225931.png]]
* Register to secondary with expires 0
* 200ok from primary with 120, which is the value set on the service parameter

![[Pasted image 20240404225940.png]]

**⇒ [Documentation](https://www.cisco.com/c/en/us/support/docs/collaboration-endpoints/unified-ip-phone-7900-series/200410-Troubleshoot-IP-Phone-Unregistration-A.html)**

	[+] Customer is going to provision 10 phones on his network. He doesn't have DHCP configured for voice, so he will do it manually. He configures "Alternate TFTP" to Yes and points 5 phones to the Publisher and 5 with the Subscriber. After some minutes only the 5 pointing to the Publisher were registered. What would be a possible reason for this? Select one: 
	
	a. Call Manager Service on the Subscriber is not running
	b. TFTP service is only running on the publisher 
	c. CM group only has Publisher node 
	d. A phone can only be registered to a Publisher

**Answer ⇒ b. TFTP service is only running on the publisher** 

	[+] Refer to the Exhibit. A call is failing to establish between two SIP devices. The called Device answers with this SDP Which SDP parameter causes the issue?
	
	![[Pasted image 20240404230315.png]]
	
	a. Opus codec should be disabled
	b. The media stream is set to sendonly
	c. The codecs are not compatible
	d. The RTP Port is set to 0

**Answer ⇒ d. The RTP Port is set to 0**

	[+] When an Administrator is troubleshooting DTMF, which kind of messages should be checked on a packet
	capture? (Choose two)
	Select one or more:
	
	a. RTP RFC2833 Events
	b. INVITE SDP
	c. 200OK
	d. NOTIFY KPML

**Answer ⇒** 

**a. RTP RFC2833 Events**
**d. NOTIFY KPML**

	[+] What is a software-based media resource that is provided by the Cisco IP Voice Media Streaming Application?
	
	Select one:
	a. IVR
	b. Annunciator
	c. Transcoder
	d. Video conference bridge

**Answer ⇒ Annunciator**

	[+] Multiple route patterns match a number. How does CUCM determine which pattern to use?
	
	Select one:
	a. the one that discards everything PreDot
	b. the one with the longest match
	c. the one with the closest match
	d. the one that comes first in numerical order

**Answer ⇒ c. the one with the closest match**

	[+] A Network Administrator deleted a user from the LDAP directory of a company. The end user is inactive LDAP
	Synchronized User in Cisco unified Communication Manager. Which steps is next to remove this user for Cisco Unified Communication Manager?
	
	Select one:
	a. User will be converted to local user and admin has to remove it manually.
	b. Execute a Manual Sync to refresh the local database and delete the user.
	c. Wait 24 hours for the garbage collect remove the user
	d. Delete the user directly from Cisco Unified Communication Manager
	e. Restart the Dirsync service after the user is deleted from LDAP directory

**Answer ⇒ c. Wait 24 hours for the garbage collect remove the user**

After the synchronization is completed, any LDAP synchronized accounts that were not set to active are permanently deleted from Unified CM when the garbage collection process runs. Garbage collection is a process that runs automatically at the fixed time of 3:15 AM, and it is not configurable.

[Documentation](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/srnd/collab10/collab10/directry.html#pgfId-1045233)

	[+] Which of the following codecs support both compressed and uncompressed data?
	Select one:
	
	a. G.729
	b. G.723.1
	c. G.722
	d. G.711

**Answer ⇒ G722**

	[+] Which of the following are the minimum required standards for H.323 compliance? (Choose four)
	Select one or more:
	a. H.221
	b. H.239
	c. H.264
	d. H.245 ⇒ Control Signaling
	e. H.225 ⇒ Call Signaling
	f. G.711d
	g. G.729 ⇒ Audio Codec
	h. G.722
	i. H.261 ⇒ Video Codec

**⇒ [Documentation](https://trueconf.com/standart-h323.html)

	[+] What causes poor voice quality and video pixelization in a video call?
	Select one:
	a. A firewall is blocking the RTP ports
	b. The QoS is configured incorrectly
	c. 1 Gbps network ports are used instead of 100 Mbps network ports
	d. Cisco Unified Communication Manager is configured to use G.711 instead of G.729


**Answer ⇒ b. The QoS is configured incorrectly**

	[+] When an application (TAPI/JTAPI or an application that directly connects to the CTIManager) fails, What will call manager do? Select one or more: 
	
	a. Restarts the CTI manager services 
	b. CTIManager Terminates the Active CTI Calls at CTI Port and CTI Route Points with an error code Network Unavailable. 
	c. CUCM tries to open a TCP connection with the secondary IP 
	d. CCM crash 
	e. The CTIManager closes the TCP connection with the application



	[+] Recently some phones were added to the cluster by the administrator. An user reported that when calling to 911 the operator sees the wrong number. The administrator found than CER was using the Default ERL but It couldnt find the phones/extensions anywhere ( non in unalocated phones, switch-ports or ip subnets) Where is likely to be the issue? Select one: 
	
	a. SNMP Communication Between CER and the Switch 
	b. CTI Communication Between CUCM and CER 
	c. SNMP Communication Between CUCM and the Switch 
	d. SNMP Communication Between CER and CUCM 
	e. AXL Communication Between CUCM and CER
	
	[+] Which statements about Music on Hold are true? (Choose 2). Select one or more: 
	
	a. CTI devices do support the multicast Music On Hold feature. 
	b. MOH supports three types of hold: End-user hold, network hold and multicast hold. 
	c. CTI devices do not support the multicast Music On Hold feature. 
	d. MOH supports two types of hold: End-user hold and network hold




	[+] The customer has configured a System Call Handler in Unity to transfer the calls to an extension using a supervised transfer. Once it's doing the supervise transfer it says 'Wait while I transfer your call' and the actual destination extension is ringing at this point. However, he cannot hear ringback or MoH. But the extension is actually ringing and he can pickup the call and then talk. The normal MoH is working in CUCM. Also he has configured the common device config and selected the MoH and applied it on the Unity Sip Trunk. For release to switch the customer can hear the ringback tone. But the requirement is to set to supervise transfer in order to hear the MoH if the extension is busy. What are probable causes for this issue? Select one: 
	
	a. MOH feature is not available during supervised transfers for the Cisco Unified CM SIP trunk integration. 
	b. Duplex Stream is required for the provider to receive the MoH RTP stream 
	c. A MTP is required for this feature to work 
	d. The service parameter "H323 Send H225 User Info Message" is set to "User Info for Call Progress Tone" and needs to be change to "Use ANN for Ring Back"
