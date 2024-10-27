Will be invoked in the following Conditions, if MRGL contains ANN

| Condition                                                                                           | Audible Announcement                                                                                                            |
| --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| An equal or higher precedence call is in progress.  MLPP only                                       | Equal or higher precedence calls have prevented the completion of your call.  Please hang up and try again.                     |
| A precedence access limitation exists.                                                              | Precedence access limitation has prevented the completion of your call.  Please hang up and try again.                          |
| Someone attempted an unauthorized precedence level.                                                 | The precedence used is not authorized for your line.  Please use and authorized precedence or ask your operator for assistance. |
| The call appears busy or the administrator did not configure the directory number for call waiting. | The number you have dialed is busy and not equipped for call waiting. Please hang up and try again.                             |
| The system cannot complete the call.                                                                | Your call cannot be completed as dialed.  Please consult your directory and call again or ask your operator for assistance.     |
| A service interruption occurred.                                                                    | A service disruption has prevented the completion of your call.  In case of emergency call your operator.                       |

Besides, if we need to signal to the that party

| Announcement | Condition                                                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| RingBack     | Blind transfer of a call established over an H.323 ICT. “Send H225 User info Message” Service Parameter must be set to Use ANN for ringback. |
| RingBack     | Blind transfer of a call established with a SIP client.                                                                                      |
| RingBack     | Blind transfer of a call established over a SIP trunk.                                                                                       |
| Barge Tone   | Before a participant joins an Ad-Hoc conference                                                                                              |
| RingBack     | Blind transfer of any type of endpoint into a conference call                                                                                |

By default for an ANN running on a server that runs CCM service supports 48 simultaneous streams, for lowers bandwidth can be decreased to 24.



``` c
84368075.001 |15:41:32.684 |AppInfo  |SIPTcp - wait_SdlSPISignal: Outgoing SIP TCP message to 172.17.16.7 on port 5060 index 281446 
[18586112,NET]
INVITE sip:7359@172.17.16.7:5060 SIP/2.0
Via: SIP/2.0/TCP 172.17.16.13:5060;branch=z9hG4bK9eb12762e1845
From: "Vashti Self" <sip:3427@172.17.16.13>;tag=6128810~10185aaa-f16c-43c6-8b11-fd1ccfbe386d-37245911
To: <sip:7359@172.17.16.7>
Date: Thu, 19 Sep 2024 19:41:32 GMT
Call-ID: 2abbc380-1f0194c2-6170e-d1011ac@172.17.16.13
Supported: timer,resource-priority,replaces
Min-SE:  1800
User-Agent: Cisco-CUCM12.5
Allow: INVITE, OPTIONS, INFO, BYE, CANCEL, ACK, PRACK, UPDATE, REFER, SUBSCRIBE, NOTIFY
CSeq: 101 INVITE
Expires: 180
Allow-Events: presence, kpml
Supported: X-cisco-srtp-fallback,X-cisco-original-called
Call-Info: <sip:172.17.16.13:5060>;method="NOTIFY;Event=telephone-event;Duration=500"
Call-Info: <urn:x-cisco-remotecc:callinfo>;x-cisco-video-traffic-class=DESKTOP
Session-ID: 212e0a8200105000a0000041d2924d45;remote=00000000000000000000000000000000
Cisco-Guid: 0716948352-0000065536-0000018669-0219156908
Session-Expires:  1800
P-Asserted-Identity: "Vashti Self" <sip:3427@172.17.16.13>
Remote-Party-ID: "Vashti Self" <sip:3427@172.17.16.13>;party=calling;screen=yes;privacy=off
Contact: <sip:3427@172.17.16.13:5060;transport=tcp>;+u.sip!devicename.ccm.cisco.com="SEP0041D2924D45"
Max-Forwards: 69
Content-Length: 0
```