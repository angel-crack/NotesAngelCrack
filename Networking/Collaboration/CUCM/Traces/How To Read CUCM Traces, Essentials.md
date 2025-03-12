Autor: Angel Ubarnes.

# ==1. How to Gather logs.==

Basically there are 3 ways to Gather Them:
-  Real-Time monitoring tool (RTMT) **⇒ COMMON WAY**
Requires windows machine with Java and reacheable to CUCM.
-  Console to SFTP
Requires SFTP server reacheability.
-  Read directly from Console
Easy, but is quite difficult when data is large.

## 1.a Real-Time monitoring tool (RTMT)


1. (Just in Case Not App Installed) Download App from CCM Admin > Application > Plugins > Find > Download "Cisco Unified Real-Time Monitoring Tool - Windows"
2. Open the Application from Windows

![[Pasted image 20240919171831.png]]

3. IP of CUCM Node

![[Pasted image 20240919171936.png]]

4. If Prompt asking for certificates appears, accept 
5. Authenticate with CUCM Application User

![[Pasted image 20240919172044.png]]

6. From the left Pane, Under System > Tools > Trace & Log Central > Double Click on "Collect Files"

![[Pasted image 20240919172431.png]]

7. On the Window that Pop-Up you will be able to see all the posible logs that you will be able to gather. In general terms, each service on CUCM has its own log associated in order to track its functionality.
8. Select "Cisco CallManager" 12th Option, that it is related to Call Manager Service (the one that handle calls), And Click on Next two times
9. From here you will be able to select the time frame for the log collection, even that the Absolute Range (select an specfic date) option is available it is always recommended to use Relative Range, which will take the "last" amount of time from now.

![[Pasted image 20240919173114.png]]

10. On Download File Directory select an specfic directory to store the logs
11. Do not change the other Options
12. Before you click on finish, recreate the issue or call that you want to analyze, once it is done then click on Finish.
13. When it finished you will see something like

![[Pasted image 20240919173557.png]]

14. On the Selected Directory you will see an structure like:
![[Pasted image 20240919173714.png]]

The amount of folders will depend on the number of nodes you have on your cluster. One folder per node, and one TraceCollectionXML associated to each folder specifing what will be the logs inside that folder. You will notice that at the end of the folder you will see a number, that will be the ID identifier for the node, this will have more sense in the future, but take that into consideration.

## 1.b Console to SFTP

Refer to: https://www.cisco.com/c/en/us/support/docs/unified-communications/unified-communications-manager-callmanager/211351-Collect-Communication-Manager-Logs-via-t.html

Basically, we will have a directory where the logs of a type of service will be stored. Let's suppose we want Cisco CallManager.

![[Pasted image 20240919174139.png]]

It shows 2 Directories. It is valid to mention that the command "file **get**" Will send the selected files to an SFTP, in this case 

```c
  file get activelog cm/trace/ccm/sdl/SDL.* 
```

Will be sending all the files inside cm/trace/ccm/sdl/ that starts with SDL.

We wont be able to select a time or collect all the logs for all the nodes, we will collect all the SDL for the node where we are log in

## 1.c Read directly from Console

We will need to get the path from the previous method, but we are gonna list all the files in that directory, first.

```c
  file list activelog cm/trace/ccm/sdl/

SDL001_100.index                        SDL001_100_000001.txt.gz
SDL001_100_000002.txt.gz                SDL001_100_000003.txt.gz
SDL001_100_000004.txt.gz                SDL001_100_000005.txt.gz
SDL001_100_000006.txt.gz                SDL001_100_000007.txt.gz
SDL001_100_000008.txt.gz                SDL001_100_000009.txt.gz
SDL001_100_000010.txt.gz                SDL001_100_000011.txt.gz
SDL001_100_000012.txt.gz                SDL001_100_000013.txt.gz
SDL001_100_000014.txt.gz                SDL001_100_000015.txt.gz
SDL001_100_000016.txt.gz                SDL001_100_000017.txt.gz
SDL001_100_000018.txt.gz                SDL001_100_000019.txt.gz
SDL001_100_000020.txt.gz                SDL001_100_000021.txt.gz
.
.
.
SDL001_100_001498.txt.gz                SDL001_100_001499.txt.gz
SDL001_100_001500.txt.gz
```

We will see that each file follow a sequence. Each file logs all the information related to CM Service from One specific time until the size of the file reachs 10MB, once that happen a new file will be created to continue logging the info.

For example, file dump will shows 10 lines at time of the file mentioned, the first line will tell us when that file was created:

``` c hl:3|Date:
admin: file dump activelog cm/trace/ccm/sdl/SDL001_100_001499.txt.gz 10

28131994.000 |21:53:46.259 |FileHead |UTC:-05:00,Date: 2024/08/12, AppName: CCM, AppId: 100, AppNodeId: 1, AppVersion: CUCM Install=11.5.1.16900-16 Build=11.5.1.16900-16 (d3f126) memlog=20, HostName: Trainingpub115, HostIPAddress: 10.207.166.103, AppStartTime: 2024/03/17 09:26:28, FileNumber: 1262, TraceVer: 3.0
28131995.000 |21:53:46.259 |SdlSig   |DbObjectCacheTimer                     |initialized                    |Db(1,100,213,1)                  |SdlTimerService(1,100,3,1)       |1,100,150,1.1^*^*                        |[T:H-H:0,N:0,L:0,V:0,Z:0,D:0]  AppCorr: 0
28131996.000 |21:53:47.265 |SdlSig   |DbObjectCacheTimer                     |initialized                    |Db(1,100,213,1)                  |SdlTimerService(1,100,3,1)       |1,100,150,1.1^*^*                        |[T:H-H:0,N:0,L:0,V:0,Z:0,D:0]  AppCorr: 0
28131997.000 |21:53:48.051 |SdlSig   |ReapOldTokenRegistrationsTimer         |wait                           |SIPStationInit(1,100,75,1)       |SdlTimerService(1,100,3,1)       |1,100,150,1.1^*^*                        |[R:H-H:1,N:0,L:0,V:0,Z:0,D:0]
28131998.000 |21:53:48.272 |SdlSig   |DbObjectCacheTimer                     |initialized                    |Db(1,100,213,1)                  |SdlTimerService(1,100,3,1)       |1,100,150,1.1^*^*                        |[T:H-H:0,N:0,L:0,V:0,Z:0,D:0]  AppCorr: 0
28131999.000 |21:53:49.290 |SdlSig   |DbObjectCacheTimer                     |initialized                    |Db(1,100,213,1)                  |SdlTimerService(1,100,3,1)       |1,100,150,1.1^*^*                        |[T:H-H:0,N:0,L:0,V:0,Z:0,D:0]  AppCorr: 0

```

Now, if we go to the next file, we see that it was created at 2024/08/13:

```c, hl:2|date
admin: file dump activelog cm/trace/ccm/sdl/SDL001_100_001500.txt.gz 10
28147301.000 |00:00:00.206 |FileHead |UTC:-05:00,Date: 2024/08/13, AppName: CCM, AppId: 100, AppNodeId: 1, AppVersion: CUCM Install=11.5.1.16900-16 Build=11.5.1.16900-16 (d3f126) memlog=20, HostName: Trainingpub115, HostIPAddress: 10.207.166.103, AppStartTime: 2024/03/17 09:26:28, FileNumber: 1263, TraceVer: 3.0
28147302.000 |00:00:00.206 |SdlSig   |DbObjectCacheTimer                     |initialized                    |Db(1,100,213,1)                  |SdlTimerService(1,100,3,1)       |1,100,150,1.1^*^*                        |[T:H-H:0,N:0,L:0,V:0,Z:0,D:0]  AppCorr: 0
28147303.000 |00:00:00.397 |AppInfo  |HttpConnection - checkConnQueue found no connData
28147304.000 |00:00:00.579 |SdlSig   |DeviceEventReceiptMonitoringTimer      |wait                           |StationInit(1,100,65,1)          |SdlTimerService(1,100,3,1)       |1,100,150,1.1^*^*                        |[R:H-H:0,N:0,L:0,V:0,Z:0,D:0]
28147305.000 |00:00:01.217 |SdlSig   |DbObjectCacheTimer                     |initialized                    |Db(1,100,213,1)                  |SdlTimerService(1,100,3,1)       |1,100,150,1.1^*^*                        |[T:H-H:0,N:0,L:0,V:0,Z:0,D:0]  AppCorr: 0
28147306.000 |00:00:01.217 |SdlStat  |Period: 6s #Lines: 60 #Bytes: 9670 Total Number of Buffers: 10000 Free LWM: 9955 Free LWM(total): 9537
28147307.000 |00:00:02.222 |SdlSig   |DbObjectCacheTimer                     |initialized                    |Db(1,100,213,1)                  |SdlTimerService(1,100,3,1)       |1,100,150,1.1^*^*                        |[T:H-H:0,N:0,L:0,V:0,Z:0,D:0]  AppCorr: 0
Press <enter> for 1 line, <space> for one page, or <q> to quit

```

So we can say that on SDL001_100_001499.txt.gz we will see all the logs for CM Service between 2024/08/12-21:53:46.259 until 2024/08/13 00:00:00.206, if our call recreation was done between those times we can collect that file either using the file get and sending the file to an SFTP server, or using the file get to see the whole file in console , or using the file tail, to see the last lines. Then you copy all the to clipboard on the SFTP client that you are using.

# ==2. How to read Logs.==

There are two ways, 

1. Using Translator X (Just shows signaling)
2. Reading on Notepad++ (shows all the information)

Let's make a simple call

Call Between 2 Devices (SIP) Register to the same CUCM Cluster

![[Pasted image 20240919181024.png]]

We are gonna make a call from 1001 to 2001, It is always important to know the call details

Calling Number: 1001
Called Number: 2001
Dialed Digits: 2001 (In this case is the same, but in case of be an external call probably there is a prefix to make the call)
Timestamp: 19 Sept 2024 - 18:15
Description: When 2001 receive the call, 1001 reject the call

Calling Number: 1001
Called Number: 2001
Dialed Digits: 2001 (In this case is the same, but in case of be an external call probably there is a prefix to make the call)
Timestamp: 19 Sept 2024 - 18:30
Description: When 2001 receive the call, 2001 answers, talk for a little bit and 2001 hang up the call

[+] We set up RTMT as above, we configure how logs gonna be collected
[+] Make the call

![[Pasted image 20240919181524.png]]

[+] Click on Finish

[+] Logs collected:

![[Pasted image 20240919181643.png]]


## 2.a Read Using Translator X

Download Tranlastor X from Here:

https://translatorx.org/downloads.html

[+] Open Translator X application and Press "Cntrl + Shift + O" check all check boxes and click on Ok

[+] Select the Directory were the logs are stored, in my case "C:\Users\aubarnes\OneDrive - Cisco\Desktop\Logs Training" and click Select Folder

[+] It will start proccesing the files, once it is done it will show you, something similar like this, then you click on "Call List" :

![[Pasted image 20240919182019.png]]


![[Pasted image 20240919183154.png]]

From Here, we can select the call, but check that only one call appears here, that's because just calls that are trying to be answered or answer appears here, that's one limitation of translator X

Anyway, selecting the call and Clicking on generate filter, close that Call List window and click on generate diagram we will see the signaling:

![[Pasted image 20240919183632.png]]

Clicking on the message we can see the sip information:

![[Pasted image 20240919183736.png]]

That will be the invite from 1001 arriving to CUCM, telling to CUCM to send the call to 2001:

![[Pasted image 20240919184354.png]]

```c, hl:5|1001,5|from: warn:6|to:,6|2001 info:1|incoming
00222499.002 |18:30:35.831 |AppInfo  |SIPTcp - wait_SdlReadRsp: Incoming SIP TCP message from 10.24.241.238 on port 53536 index 800 with 1456 bytes:
[9957,NET]
INVITE sip:2001@10.207.166.103 SIP/2.0
Via: SIP/2.0/TCP 10.24.241.238:53536;branch=z9hG4bK000023a3
From: "Maicao Line 1" <sip:1001@10.207.166.103>;tag=002b677ead6a001000005818-00003482
To: <sip:2001@10.207.166.103>
Call-ID: 002b677e-ad6a0004-00005146-00002bf9@10.24.241.238
Max-Forwards: 70
Date: Thu, 19 Sep 2024 23:30:35 GMT
CSeq: 101 INVITE
User-Agent: Cisco-SIPIPCommunicator/9.1.1
Contact: <sip:a102cd69-f435-4c67-aa01-ccbf361c83eb@10.24.241.238:53536;transport=tcp>
Expires: 180
Accept: application/sdp
Allow: ACK,BYE,CANCEL,INVITE,NOTIFY,OPTIONS,REFER,REGISTER,UPDATE,SUBSCRIBE,INFO
Remote-Party-ID: "Maicao Line 1" <sip:1001@10.207.166.103>;party=calling;id-type=subscriber;privacy=off;screen=yes
Supported: replaces,join,sdp-anat,norefersub,extended-refer,X-cisco-callinfo,X-cisco-serviceuri,X-cisco-escapecodes,X-cisco-service-control,X-cisco-srtp-fallback,X-cisco-monrec,X-cisco-config,X-cisco-sis-5.1.0,X-cisco-xsi-8.5.1
Allow-Events: kpml,dialog
Content-Length: 379
Content-Type: application/sdp
Content-Disposition: session;handling=optional
v=0
o=Cisco-SIPUA 21908 0 IN IP4 10.24.241.238
s=SIP Call
t=0 0
m=audio 19904 RTP/AVP 0 8 18 9 116 124 101
c=IN IP4 10.24.241.238
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:18 G729/8000
a=fmtp:18 annexb=no
a=rtpmap:9 G722/8000
a=rtpmap:116 iLBC/8000
a=fmtp:116 mode=20
a=rtpmap:124 ISAC/16000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-15
a=sendrecv


Timestamp: 3809615435831
UTC Timestamp:3809633435831
Source Filename: SDL001_100_000280.txt.gz
```

And SIP Invite Messsage from CUCM to 2001:

![[Pasted image 20240919184449.png]]

```c hl:5|1001,5|from: warn:6|to:,6|2001,1|outgoing
00222572.001 |18:30:35.836 |AppInfo  |SIPTcp - wait_SdlSPISignal: Outgoing SIP TCP message to 10.24.241.238 on port 53443 index 795 
[9959,NET]
INVITE sip:b6475abd-9412-4e11-8f7b-53be2366b6e1@10.24.241.238:53443;transport=tcp SIP/2.0
Via: SIP/2.0/TCP 10.207.166.103:5060;branch=z9hG4bK95e158f286f
From: "Maicao Line 1" <sip:1001@10.207.166.103>;tag=5436~6f9c10b0-08fe-4670-8b1e-a51c1b40ef5a-28109328
To: <sip:2001@10.207.166.103>
Date: Thu, 19 Sep 2024 23:30:35 GMT
Call-ID: 2a332500-6ec1b41b-95d-67a6cf0a@10.207.166.103
Supported: timer,resource-priority,replaces
Min-SE:  1800
User-Agent: Cisco-CUCM11.5
Allow: INVITE, OPTIONS, INFO, BYE, CANCEL, ACK, PRACK, UPDATE, REFER, SUBSCRIBE, NOTIFY
CSeq: 101 INVITE
Expires: 180
Allow-Events: presence
Call-Info: <urn:x-cisco-remotecc:callinfo>; security= Unknown; orientation= from; gci= 1-5004; isVoip; call-instance= 1
Send-Info: conference, x-cisco-conference
Alert-Info: <file://Bellcore-dr1/>
Session-ID: 8e677fcde88ae06fd8ee10d584aa5435;remote=00000000000000000000000000000000
Remote-Party-ID: "Maicao Line 1" <sip:1001@10.207.166.103;x-cisco-callback-number=1001>;party=calling;screen=yes;privacy=off
Contact: <sip:1001@10.207.166.103:5060;transport=tcp>
Max-Forwards: 69
Content-Length: 0



Timestamp: 3809615435836
UTC Timestamp:3809633435836
Source Filename: SDL001_100_000280.txt.gz
```

[+] This is a useful tool to check the signaling of a call, sometimes we do not need to do a deep call analysis, for example when we see the call is routed outside of CUCM to a GW, and we do see a 603 Decline or 503 Service Unavaiable coming from GW, the analysis should be done on the other side insted of CUCM.

## 2.c Reading SDL Files from CUCM Traces (Simple Interior Call with SIP protocol)

This is a highly summarize diagram of what CUCM does when recieve a SIP Invite, it is just the basic logic to follow, it can be a little bit complex in the reality.

![[Pasted image 20240921134320.png]]

[+] Most of the times on troubleshooting scenarios we will have a calling Example to be Analyzed, customer will provide details as:

a. : SR 697953357 : Unable to dial out on this number  415-353-3395.

*Hello Angel,*

*As per your request we have collected the logs and  uploaded the file as well.*

*Here is the following information :-*

1. *4155147063  and 10.73.14.38*
2. *4153533395*
3. *2.50 pm*
4. *IP Address of gateway :- 10.1.4.63 and 10.1.0.137*

*Please let me know if you have any query.*

b.  SR 697554470 : Calls forwarded to other extensions have my extension

*I am currently uploading the logs for 6/27/24.*
*The call details are*
*Date/time: 6/27/24 06:32:20*
*Calling: 702-539-5334*
*Called: 702-229-6843*
*Redirect to: 702-498-9078*
*Result: got fast busy*

c. SR 697409711 : Logical partition policy review

*Hi Angel*

*We did some tests today and uploaded logs.*
*Time stamps : 13:02 IST.*
*Called Party: + 912240435233*
*Calling number: +31203577401*
*Conference : +918123232112*
*Conference failed.*

*Thank you*

Sometimes can be really complex.

d. SR 697992087 :  Switchboard number calls are failing

*Transfer is still failing for users Reference Call:* 
*Time: 12:07:22* 
*Calling: 447789432904* 
*Called: 84462221*
*Call should be transferred to :875520232 but transfer is not happening.* 

*Call Flow: SBC 10.200.158.7 ---> SME A --> XLNP Cluster --> SME B --> XAMR Cluster ---> CTI Application ARC Operators --> Agent transfer the call back to users on SME --> XLNP (Leaf)*

For our case, let's start with our 2 sample calls:

========================

[+] a. When 2001 receive the call, 2001 answers, talk for a little bit and 2001 hang up the call
Calling Number: 1001
Called Number: 2001
Dialed Digits: 2001 (In this case is the same, but in case of be an external call probably there is a prefix to make the call)
Timestamp: 19 Sept 2024 - 18:30

[+] b. When 2001 receive the call, 1001 reject the call
Calling Number: 1001
Called Number: 2001
Dialed Digits: 2001 (In this case is the same, but in case of be an external call probably there is a prefix to make the call)
Timestamp: 19 Sept 2024 - 18:15

========================

So, where to start digging? I will highly and strongly recommend to check if the call exists on the logs. It can happen that customes get the logs and the call is not presented.

1. Basically, we are goona check if CUCM perform the digit analysis after receiving the call, at this point:

![[Pasted image 20240921141230.png]]

We are gonna look for the following Regex (Regex: Basically a way to match all the words that matches and structure, so we will look for all the dates instead of search an specific date) on all the logs:

```r
.+Digit analysis\:.+fqcn=(.+?(?=")).+cn=(.+?(?=")).+,plv=.+, dd=(.+?(?="))",dac
```

On notepad++ press "Ctrl + Shift + F", that will prompt you a window with some options, make sure you have the same configure on your side:

![[Pasted image 20240921141544.png]]

Note: The regex will be on the "Find What" field, Directory will be where the customer logs are saved on your machine and do not forget to check Regular Expresion checkbox.

2. Check the results, we will have 

![[Pasted image 20240921142009.png]]

we will proceed copy all the results by clicking on Select All and pressing "Ctrl + C", inmediately press "Ctrl + N" to create a new blank file on NotePad++ and paste all the results.

![[Pasted image 20240921142119.png]]


[+] Once we paste it, it will look like:

![[Pasted image 20240921142235.png]]

[+] Every of those lines represent an analysis that CUCM does based on digits that were sent to it. So, there are mainlyn three parameters in every single line that tell us important information:

1. fqcn="1001" ⇒ Stands for: FullyQualifiedCallingNumber
2. cn="1001" ⇒ Stands for: Calling Number
3. dd=" ⇒ Stands for: Dialed Digits

Item 1 and 2 refers specifically to the Calling Number that we are looking for, not necessarily they will match, and it is beyond of this documentation explain why you just need to check if your calling number is presented at least on one of those 2.

Item 3, refers to the what Customer dialed to perform the call, so it is in some way the called number. If you receive some info from customer like:

Calling 1001 and Called 3000

But, 3000 is a line on outside of his cluster and customer needs to dialed 93000 in order to hit 9.XXXX that hits a trunk, you will see on dd="93000" instead of "3000"

So, please, take that into consideration at the moment of filtering.

[+] Once we have pasted the results on a nodepad++, we will press "Ctrl + F" in order to find just in that file we created.

![[Pasted image 20240921143315.png]]

We will look 1001 and click on Find all in Current Document

![[Pasted image 20240921143410.png]]

We see that it matches fqcn and cn, so, basically those are two calls that 1001 its attempting to do.

if we scroll horizontally until we reach the end. we will see where it is dialing: 

![[Pasted image 20240921143550.png]]

both results are calls analyzed on CUCM from 1001 to 2001

Now, checking the timestamps we see that one analysis was done at 18:15:08.147 and the other at 18:30:35.833, from our call details we know that one call was done around 18:15 and the other around 18:30, so, we can conclude that those two lines belongs to out calls, respectively.

If we look for the first call at 18:15 thorugh all the files, "Cntrl + shift + F"

```c hl:1|fqcn,1|cn="1001",dd="2001",digit,analysis
18:15:08.147 |AppInfo  |Digit analysis: match(pi="2", fqcn="1001", cn="1001",plv="5", pss="PhonesInternalHP_Partition", TodFilteredPss="PhonesInternalHP_Partition", dd="2001",dac="1")
```

![[Pasted image 20240921144034.png]]

it will found one result, always:

![[Pasted image 20240921144052.png]]

double clicking on it, will take us to the file that contains the match.

will see somthing like:

![[Pasted image 20240921144202.png]]

Then, we are going to inmediately select the following process, this one will populated all the logs that are related to this call:

![[Pasted image 20240921144350.png]]

Right click and highlight all the occurrences

![[Pasted image 20240921144427.png]]


So, we will noticed that it shows like:

![[Pasted image 20240921144504.png]]


For any of the two lines where Cdcc process is highlighted we are gonna scroll horizontly until we found the CI:

![[Pasted image 20240921144613.png]]


CI stands for CallIdentifier and it is a number that identifies de call leg between the 1001 to CUCM, we are gonna highlighted as well with blue. It identifies this portion of the call

![[Pasted image 20240921145017.png]]

So, we will refer to it from now as CI for Party A, of course, it exists the other CI for Party B, that will represent the information for the transaction between CUCM and 2001.

[+] looking for the Cdcc on all files

![[Pasted image 20240921145202.png]]

We are gonna see results like, and we are gonna double clikc on the one that says "Created":

![[Pasted image 20240921145249.png]]

You will see something like:

![[Pasted image 20240921151847.png]]

You will be scrolling up until you find a trying message:

![[Pasted image 20240921151955.png]]

Select the whole Call-ID and Search on all the files:

![[Pasted image 20240921152024.png]]

Double Clicking on the first result:

That will be the invite

![[Pasted image 20240921152052.png]]

[+] Second message will be the above trying.

[+] Third will be a Ringing 

![[Pasted image 20240921152134.png]]

[+] 4th message is a Cancel That goes from 1001 sends to CUCM, check that the first line says "Incoming SIP TCP message from X.X.X.X" this is the phone line 1001 finishing the call.

![[Pasted image 20240921152201.png]]

[+] Up to this point you have seen everything related to the Call leg between 1001 and CUCM, but how is CUCM doing the logic we mentioned before?

[+] Let's go back to the Digit Analysis:

![[Pasted image 20240921144350.png]]

