Author: Angel Ubarnes (aubarnes)

[+] GeoLocation is a description of the physical geographical area where something currently exists.

In this case will be the segmentation for each city.

![[Pasted image 20240910185127.png]]


[+] Geolocation assigns the location details to devices like IP phone, SIP trunk, Inter CLuster trunk (ICT) Gateway etc. 
[+] RFC 4119 specifies 17 Civic Location elements.

```

   +----------------------+----------------------+---------------------+
   | Label                | Description          | Example             |
   +----------------------+----------------------+---------------------+
   | country              | The country is       | US                  |
   |                      | identified by the    |                     |
   |                      | two-letter ISO 3166  |                     |
   |                      | code.                |                     |
   |                      |                      |                     |
   | A1                   | national             | New York            |
   |                      | subdivisions (state, |                     |
   |                      | region, province,    |                     |
   |                      | prefecture)          |                     |
   |                      |                      |                     |
   | A2                   | county, parish, gun  | King's County       |
   |                      | (JP), district (IN)  |                     |
   |                      |                      |                     |
   | A3                   | city, township, shi  | New York            |
   |                      | (JP)                 |                     |
   |                      |                      |                     |
   | A4                   | city division,       | Manhattan           |
   |                      | borough, city        |                     |
   |                      | district, ward, chou |                     |
   |                      | (JP)                 |                     |
   |                      |                      |                     |
   | A5                   | neighborhood, block  | Morningside Heights |
   |                      |                      |                     |
   | A6                   | street               | Broadway            |
   |                      |                      |                     |
   | PRD                  | Leading street       | N, W                |
   |                      | direction            |                     |
   |                      |                      |                     |
   | POD                  | Trailing street      | SW                  |
   |                      | suffix               |                     |
   |                      |                      |                     |
   | STS                  | Street suffix        | Avenue, Platz,      |
   |                      |                      | Street              |
   |                      |                      |                     |
   | HNO                  | House number,        | 123                 |
   |                      | numeric part only.   |                     |
   |                      |                      |                     |
   | HNS                  | House number suffix  | A, 1/2              |
   |                      |                      |                     |
   | LMK                  | Landmark or vanity   | Low Library         |
   |                      | address              |                     |
   |                      |                      |                     |
   | LOC                  | Additional location  | Room 543            |
   |                      | information          |                     |
   |                      |                      |                     |
   | FLR                  | Floor                | 5                   |
   |                      |                      |                     |
   | NAM                  | Name (residence,     | Joe's Barbershop    |
   |                      | business or office   |                     |
   |                      | occupant)            |                     |
   |                      |                      |                     |
   | PC                   | Postal code          | 10027-0401          |
   +----------------------+----------------------+---------------------+
```

[+] UCM Logical partitioning feature implemented the manual configuration of these 17 fields/elements.
[+] Geolocation Filter is a rule to select certain fields of Geolocation in order to construct Geolocation String.
[+] An Identifier that is identifier constructed from a combination of Geolocation, Filter & Device type. This identifier is used to compare against LP and call would be allowed or denied.

![[Pasted image 20240910190218.png]]

The device has a geolocation, then a filter is applied to extract some elements and finally depends if the device is Trunk or GW, Device Type will be border, any other will be Internal. Then we construct the string in backwards:

[+] A SIP trunk in CUCM can be logically represented as **Border:Country=CO:A1=ATL**
[+] Geolocations normally have all 17 fields configured and would possibly be unique for each UCM device in a cluster. So, configure policies between Geolocations may be a lot of overhead for an Administrator

**Configuration Example**

Considering the following topology

![[Pasted image 20240911133650.png]]

Let's create 4 Device Pools

[+] Device Pool 

```
╔══════════════════╤═════════════════════════════╤═════════════════════════════╗
║ Device Pool Name │ Call Manager 10.207.166.103 │ Call Manager 10.207.166.104 ║
╠══════════════════╪═════════════════════════════╪═════════════════════════════╣
║ Barranquilla_DP  │ x                           │                             ║
╟──────────────────┼─────────────────────────────┼─────────────────────────────╢
║ Maicao_DP        │ x                           │                             ║
╟──────────────────┼─────────────────────────────┼─────────────────────────────╢
║ Caracas_DP       │                             │ x                           ║
╟──────────────────┼─────────────────────────────┼─────────────────────────────╢
║ Maracaibo_DP     │                             │ x                           ║
╚══════════════════╧═════════════════════════════╧═════════════════════════════╝
```

[+] on CUCM 103

![[Pasted image 20240911135401.png]]

[+] on CUCM 104

![[Pasted image 20240911135414.png]]

[+] Let's gonna create the Geolocation on each node, take into consideration that each Geolocation is a description of the physical geographical area where something currently exists.

System > Geolocation Configuration

Barranquilla_GL
![[Pasted image 20240911143538.png]]

Guajira_GL

![[Pasted image 20240911161147.png]]


Zulia_GL
![[Pasted image 20240911161211.png]]

Capital_GL
![[Pasted image 20240911161246.png]]


Or you can do it with the following SQL 

``` SQL
run sql insert into geolocation (A2, A1, Pod, Hno, Lmk, Description, A4, A3, A6, Hns, A5, Nam, Pc, Sts, Country, Prd, Flr, Name, Loc) values ('', 'Atlantico', 'Calle', '24', '', 'Devices Inside of Barranquilla Area', '', 'Barranquilla', '26', '', 'El Por Fin', 'Angel''s House', '080013', '84', 'CO', '', '1', 'Barranquilla_GL', 'Room 3')

run sql insert into geolocation (A2, A1, Pod, Hno, Lmk, Description, A4, A3, A6, Hns, A5, Nam, Pc, Sts, Country, Prd, Flr, Name, Loc) values ('', 'Guajira', 'Calle', '1', '', 'Devices Inside of Maicao Area', '', 'Maicao', '5', '', 'Finca Paz', 'Carlos''s House', '040021', '84', 'CO', '', '1', 'Maicao_GL', 'Lote 12')

run sql insert into geolocation (A2, A1, Pod, Hno, Lmk, Description, A4, A3, A6, Hns, A5, Nam, Pc, Sts, Country, Prd, Flr, Name, Loc) values ('', 'Zulia', 'Calle', '1', '', 'Devices Inside of Maracaibo Area', '', 'Maracaibo', '', '', '', 'Raul''s House', '050022', '14', 'VE', '', '1', 'Maracaibo_GL', '')

run sql insert into geolocation (A2, A1, Pod, Hno, Lmk, Description, A4, A3, A6, Hns, A5, Nam, Pc, Sts, Country, Prd, Flr, Name, Loc) values ('', 'Capital', 'Calle', '1', '', 'Devices Inside of Caracas Area', '', 'Caracas', '', '', '', 'Andres''s House', '020021', '81', 'VE', '', '1', 'Caracas_GL', '')

```


![[Pasted image 20240911165020.png]]

We create the 4 GL in each Node.

[+] As you can see Geolocation can be as granular as you want, and it can be very complex to create rules between one geolocation and another, for example, if we want to have the geolocation policy for 100 users that are remote, would be 100 geolocations, if we have 10GW and want to allow with some of them based on location, will be over 1000 polilicies. So, we can filter information of the geolocation to construct the Geolocation string and hava few rules.

We want to construct the following policy, for these calls:

```
╔════════════════╤══════════════╤════════╤═════════╤═══════════╗
║ Calling/Called │ Barranquilla │ Maicao │ Caracas │ Maracaibo ║
╠════════════════╪══════════════╪════════╪═════════╪═══════════╣
║ Barranquilla   │              │        │ Allow   │ Allow     ║
╟────────────────┼──────────────┼────────┼─────────┼───────────╢
║ Maicao         │              │        │ Allow   │ Deny      ║
╟────────────────┼──────────────┼────────┼─────────┼───────────╢
║ Caracas        │ Allow        │ Deny   │         │           ║
╟────────────────┼──────────────┼────────┼─────────┼───────────╢
║ Maracaibo      │ Deny         │ Deny   │         │           ║
╚════════════════╧══════════════╧════════╧═════════╧═══════════╝
```

As we are use City, the following information for the geolocation is the one that we need:

country A1 A3 

Under System > Geolocation Filter

![[Pasted image 20240911164202.png]]

[+] In our example, we are going to add geolocation and geolocation filter to respective Device Pools

![[Pasted image 20240911165159.png]]

On Barranquilla_DP

![[Pasted image 20240911164605.png]]

We should do the same on all the other Device Pools, with the following SQL query we can know what would be the geolocation and geolocation filter for all the device pools

CUCM 103 (Colombia)

``` sql hl:9,8
run sql select dp.name, gl.name, glf.name from devicepool as dp left join geolocation as gl on gl.pkid = dp.fkgeolocation left join geolocationfilter as glf on glf.pkid = dp.fkgeolocationfilter_lp

name                name                name
=================== =================== =======================================
Default             NULL                NULL
PuertoColombiaLines CUCM 103 Geoocation StandardFilter
Beto_CER            NULL                NULL
Barranquilla_DP     Barranquilla_GL     GeolocationForFilter_Country_State_City
Maicao_DP           Maicao_GL           GeolocationForFilter_Country_State_City
```

CUCM 104 (Venezuela)

``` sql hl:5,6
run sql select dp.name, gl.name, glf.name from devicepool as dp left join geolocation as gl on gl.pkid = dp.fkgeolocation left join geolocationfilter as glf on glf.pkid = dp.fkgeolocationfilter_lp
name         name         name
============ ============ =======================================
Default      NULL         NULL
Maracaibo_DP Maracaibo_GL GeolocationForFilter_Country_State_City
Caracas_DP   Caracas_GL   GeolocationForFilter_Country_State_City
```

We can check what geolocation has the every device on CUCM

```sql hl:35,36
run sql select dp.name, gl.name, glf.name from device as dp left join geolocation as gl on gl.pkid = dp.fkgeolocation left join geolocationfilter as glf on glf.pkid = dp.fkgeolocationfilter_lp
name                                               name               name
================================================== ================== ==============
SAMPLE DEVICE TEMPLATE WITH TAG USAGE EXAMPLES     NULL               NULL
AUTO-REGISTRATION TEMPLATE                         NULL               NULL
MTP_2                                              NULL               NULL
CFB_2                                              NULL               NULL
ANN_2                                              NULL               NULL
MOH_2                                              NULL               NULL
IVR_2                                              NULL               NULL
Test_CTI_RP                                        NULL               NULL
CIPC_TemTest                                       NULL               NULL
ANAAFFFFFFFAFFF                                    NULL               NULL
HL_test_1                                          NULL               NULL
RL-Cajon                                           NULL               NULL
XCoder_CPH                                         NULL               NULL
phone_8865                                         NULL               NULL
ModelProfileFor76acac08-a4d4-04c1-d85e-a2464a2867b NULL               NULL
HL training                                        NULL               NULL
Training_RL                                        NULL               NULL
EM1                                                NULL               NULL
EM2                                                NULL               NULL
ModelProfileFor5d9ac866-bcb4-57ac-868e-ff5ae4da05e NULL               NULL
RP_911                                             NULL               NULL
RP_912                                             NULL               NULL
RP_913                                             NULL               NULL
Beto_profile                                       NULL               NULL
Test Webex                                         NULL               NULL
AALN/S1/SU0/0@ALN/domain.com                       NULL               NULL
cd                                                 NULL               NULL
ModelProfileForfea30f3c-a46d-2b01-dcef-57f945ac0ad NULL               NULL
LabTrunk_10.207.166.106                            NULL               NULL
CiscoUM1-VI1                                       NULL               NULL
dummy_Trunk                                        NULL               NULL
AAAAAAAAAAAAAAA                                    NULL               NULL
BBBBBBBBBBBBBBB                                    NULL               NULL
RDP_RACO                                           NULL               NULL
To_10.207.166.104                                  GeolocationCUCM104 StandardFilter
HL LineGroupA LineGroupB                           NULL               NULL

```

Even that our devices are on that device pool they do not have any geolocation, this is due to that we can add geolocation add device level

[+] Phones

![[Pasted image 20240911171107.png]]

[+] Trunk

![[Pasted image 20240911171133.png]]

We can add here, it is important to check Send geolocation information.

[+] At enterprise level (sytem > enterprise parameter) we should enable Geolocation Logical Partitioning and add the Default Geolocation for devices that do not have configure either on the phone or at device pool level.

![[Pasted image 20240911171704.png]]


[+] Geolocation CUCM 103

``` sql
run sql insert into geolocation (A2, A1, Pod, Hno, Lmk, Description, A4, A3, A6, Hns, A5, Nam, Pc, Sts, Country, Prd, Flr, Name, Loc) values ('', '', '', '', '', 'Devices Inside of CUCM 103 - Colombia', '', '', '', '', '', '', '', '', 'CO', '', '', 'CUCM 103 Geoocation', '')
```

For CUCM 104 

``` sql
run sql insert into geolocation (A2, A1, Pod, Hno, Lmk, Description, A4, A3, A6, Hns, A5, Nam, Pc, Sts, Country, Prd, Flr, Name, Loc) values ('', '', '', '', '', 'Devices Inside of CUCM 104 - Venezuela', '', '', '', '', '', '', '', '', 'CO', '', '', 'CUCM 104 Geoocation', '')
```


![[Pasted image 20240911171616.png]]


[+] Referencing to our table, let's start with policy for Maicao>Maracaibo

``` hl:6
╔════════════════╤══════════════╤════════╤═════════╤═══════════╗
║ Calling/Called │ Barranquilla │ Maicao │ Caracas │ Maracaibo ║
╠════════════════╪══════════════╪════════╪═════════╪═══════════╣
║ Barranquilla   │              │        │ Allow   │ Allow     ║
╟────────────────┼──────────────┼────────┼─────────┼───────────╢
║ Maicao         │              │        │ Allow   │ Deny      ║
╟────────────────┼──────────────┼────────┼─────────┼───────────╢
║ Caracas        │ Allow        │ Deny   │         │           ║
╟────────────────┼──────────────┼────────┼─────────┼───────────╢
║ Maracaibo      │ Deny         │ Deny   │         │           ║
╚════════════════╧══════════════╧════════╧═════════╧═══════════╝
```

[+] Let's Make a Test Call from Maicao to Maracaibo which will 1001 dialing 93000. 

![[Pasted image 20240912151136.png]]

We are able to make the call, actually, we are able to make it in reverse 3000 > 91001:

![[Pasted image 20240912151329.png]]

[+] We are also able to call from Maicao (1001) to Caracas (4000), both ways:

![[Pasted image 20240912151553.png]]

![[Pasted image 20240912152452.png]]

[+] Let's deny calls from Maicao to Maracaibo which will be 1001 dialing 93000 using geolocation. 

1. Over: Call Routing > Logical Partitioning Policy Configuration > Add New
2. Here is the trick number 1, Policy should be basically what the geolocation string will match to be identified:

![[Pasted image 20240912154442.png]]

Here on every dropdown we will have available all the information defined for each element on our geolocations.

[+] It can also be create with this SQL query:

``` sql
run sql insert into geolocationpolicy (A2, A1, Pod, Hno, Lmk, Description, A4, A3, A6, Hns, A5, Nam, Pc, Sts, Country, Prd, Flr, Name, Loc) values ('', 'Guajira', '', '', '', 'Matches Devices located in Maicao', '', 'Maicao', '', '', '', '', '', '', 'CO', '', '', 'MaicaoPolicy', '')
```

[+] We are going to create another one for Maracaibo:

``` sql
run sql insert into geolocationpolicy (A2, A1, Pod, Hno, Lmk, Description, A4, A3, A6, Hns, A5, Nam, Pc, Sts, Country, Prd, Flr, Name, Loc) values ('', 'Zylia', '', '', '', 'Matches Devices located in Maracaibo', '', 'Maracaibo', '', '', '', '', '', '', 'VE', '', '', 'MaracaiboPolicy', '')
```

[+] Now, what logic says is to create a Relationship between Internal Maicao to Border Maracaibo and Deny those, calls

![[Pasted image 20240912154744.png]]

![[Pasted image 20240912154800.png]]

[+] However, this won't work (considering we are doing this on CUCM 103 (Colombia) ), let's check why

[+] Invite arriving to CUCM (check geolocation info is not here)

``` c
00004995.002 |16:25:42.252 |AppInfo  |SIPTcp - wait_SdlReadRsp: Incoming SIP TCP message from 10.82.229.57 on port 53250 index 6 with 1452 bytes:
[120,NET]
INVITE sip:93000@10.207.166.103 SIP/2.0
Via: SIP/2.0/TCP 10.82.229.57:53250;branch=z9hG4bK00001714
From: "Maicao Line 1" <sip:1001@10.207.166.103>;tag=002b677ead6a05f0000018bd-000067e6
To: <sip:93000@10.207.166.103>
Call-ID: 002b677e-ad6a001a-00006fcf-00006494@10.82.229.57
Max-Forwards: 70
Date: Thu, 12 Sep 2024 21:25:42 GMT
CSeq: 101 INVITE
User-Agent: Cisco-SIPIPCommunicator/9.1.1
Contact: <sip:a102cd69-f435-4c67-aa01-ccbf361c83eb@10.82.229.57:53250;transport=tcp>
Expires: 180
Accept: application/sdp
Allow: ACK,BYE,CANCEL,INVITE,NOTIFY,OPTIONS,REFER,REGISTER,UPDATE,SUBSCRIBE,INFO
Remote-Party-ID: "Maicao Line 1" <sip:1001@10.207.166.103>;party=calling;id-type=subscriber;privacy=off;screen=yes
Supported: replaces,join,sdp-anat,norefersub,extended-refer,X-cisco-callinfo,X-cisco-serviceuri,X-cisco-escapecodes,X-cisco-service-control,X-cisco-srtp-fallback,X-cisco-monrec,X-cisco-config,X-cisco-sis-5.1.0,X-cisco-xsi-8.5.1
Allow-Events: kpml,dialog
Content-Length: 376
Content-Type: application/sdp
Content-Disposition: session;handling=optional

v=0
o=Cisco-SIPUA 6865 0 IN IP4 10.82.229.57
s=SIP Call
t=0 0
m=audio 27544 RTP/AVP 0 8 18 9 116 124 101
c=IN IP4 10.82.229.57
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
```

[+] Digit Analysis 1001 > 93000, hits 9.XXXX > trunk: To_10.207.166.104 :

``` c
00005023.012 |16:25:42.254 |AppInfo  |Digit analysis: match(pi="2", fqcn="1001", cn="1001",plv="5", pss="PhonesInternalHP_Partition", TodFilteredPss="PhonesInternalHP_Partition", dd="93000",dac="1")
00005023.013 |16:25:42.254 |AppInfo  |Digit analysis: analysis results
00005023.014 |16:25:42.254 |AppInfo  ||PretransformCallingPartyNumber=1001
|CallingPartyNumber=1001
|DialingPartition=PhonesInternalHP_Partition
|DialingPattern=9.XXXX
|FullyQualifiedCalledPartyNumber=93000

00005024.003 |16:25:42.254 |AppInfo  |SMDMSharedData::findLocalDevice - Name=To_10.207.166.104 
```

[+] With CI of this leg we can see that the proccess for analyze geolocation initiate:

``` c hl:aCI,bCI
00005030.001 |16:25:42.254 |AppInfo  |LPSession -await_associate_PolicyAssociateReq, aCi[26044483], bCi[26044484]
```

[+] We will look for the following string and replacing ci for one of the above to check what CUCM do for geolocation

```c
LPSession -wait_PolicyRegisterReq, ci[26044484]
```

NOTE: CUCM will look first for the Called Party, in this case will be for the trunk with CI 26044484.

GeolocInfoA will be Phone(Calling) and GeolocInfoB will be the trunk(Called), devType = 4 ⇒ Interior, devType= 8 ⇒ Border

``` c hl:geolocinfoA,geolocinfob
00005043.002 |16:25:42.255 |AppInfo  |LPPolicyManager -getLogicalPartitionPolicy, GeolocInfoA[pkid=9d0b46fb-f60f-421e-b8cd-58cde014ff05, filter=78beb8b9-ff60-d1ed-2623-66677b6848c1, val=, devType=4]
00005043.003 |16:25:42.255 |AppInfo  |LPPolicyManager -getLogicalPartitionPolicy, GeolocInfoB[pkid=, filter=, val=, devType=8]
```

[+] For the Pkid on GeolocInfoA 9d0b46fb-f60f-421e-b8cd-58cde014ff05 we can see the data:

```sql
run sql select * from geolocation where pkid = "9d0b46fb-f60f-421e-b8cd-58cde014ff05"

pkid                                 name      country description                   a1      a2 a3     a4 a5        a6 prd pod   sts hno hns lmk loc     flr nam            pc
==================================== ========= ======= ============================= ======= == ====== == ========= == === ===== === === === === ======= === ============== ======
9d0b46fb-f60f-421e-b8cd-58cde014ff05 Maicao_GL CO      Devices Inside of Maicao Area Guajira    Maicao    Finca Paz 5      Calle 84  1           Lote 12 1   Carlos's House 040021
```

[+] For the filter

``` sql
run sql select * from geolocationfilter where pkid = "78beb8b9-ff60-d1ed-2623-66677b6848c1"

pkid                                 name                                    description usecountry usea1 usea2 usea3 usea4 usea5 usea6 useprd usepod usests usehno usehns uselmk useloc useflr usenam usepc
==================================== ======================================= =========== ========== ===== ===== ===== ===== ===== ===== ====== ====== ====== ====== ====== ====== ====== ====== ====== =====
78beb8b9-ff60-d1ed-2623-66677b6848c1 GeolocationForFilter_Country_State_City Filter to   t          t     f     t     f     f     f     f      f      f      f      f      f      f      f      f      f
```

Notice how GeolocInfoB does not found any geolocation because we do not have anything configure on that trunk as Geolocation:

![[Pasted image 20240912164144.png]]

We just enable to send Geolocation info

In this case, CUCM will use for the trunk the Geolocation default configure on CUCM Enterprise parameters

![[Pasted image 20240912164245.png]]

[+] Then we see, check how on GeolocNamValPair printlist show us the info for the Geolocation string:

```c hl:"GeolocNamValPair"
00005043.004 |16:25:42.255 |AppInfo  |GeolocInfo -getGeolocDetailAndFilter
00005043.005 |16:25:42.255 |AppInfo  |GeolocInfo -readGeolocationRecord: geolocPkid[9d0b46fb-f60f-421e-b8cd-58cde014ff05], values c[CO], a1[Guajira], a2[], a3[Maicao], a4[], a5[Finca Paz], a6[5], prd[], pod[Calle], sts[84]. hno[1], hns[], lmk[], loc[Lote 12], flr[1], nam[Carlos's House], pc[040021]

00005043.006 |16:25:42.255 |AppInfo  |GeolocFilterInfo -readGeolocationFilterRecord: geolocPkid[78beb8b9-ff60-d1ed-2623-66677b6848c1], values c[1], a1[1], a2[0], a3[1], a4[0], a5[0], a6[0], prd[0], pod[0], sts[0]. hno[0], hns[0], lmk[0], loc[0], flr[0], nam[0], pc[0]

00005043.007 |16:25:42.255 |AppInfo  |GeolocNamValPair -printList: country = CO, A1 = Guajira, A3 = Maicao,
```

[+] Then for the Trunk, check it will use Default Geolocation and Logical Partitioning Default Filter

```c  hl:"GeolocNamValPair"
00005043.008 |16:25:42.255 |AppInfo  |GeolocInfo -getGeolocDetailAndFilter
00005043.009 |16:25:42.255 |AppInfo  |GeolocInfo -readGeolocationRecord: geolocPkid[bcea7eab-a22a-4269-ad3c-d882df3c1061], values c[CO], a1[], a2[], a3[], a4[], a5[], a6[], prd[], pod[], sts[]. hno[], hns[], lmk[], loc[], flr[], nam[], pc[]

00005043.010 |16:25:42.255 |AppInfo  |GeolocFilterInfo -readGeolocationFilterRecord: geolocPkid[71500e05-372f-30bd-ae2d-c2f1ae738c28], values c[1], a1[0], a2[0], a3[0], a4[0], a5[0], a6[0], prd[0], pod[0], sts[0]. hno[0], hns[0], lmk[0], loc[0], flr[0], nam[0], pc[0]

00005043.011 |16:25:42.255 |AppInfo  |GeolocNamValPair -printList: country = CO,
```

[+] We will see a summary then:

```c
00005043.013 |16:25:42.255 |AppInfo  |LogicalPolicyTree -searchPolicy devTypeA[Interior], devTypeB[Border]
00005043.014 |16:25:42.255 |AppInfo  |GeolocNamValPair -printList: country = CO, A1 = Guajira, A3 = Maicao,
00005043.015 |16:25:42.255 |AppInfo  |GeolocNamValPair -printList: country = CO,
```

[+] Note: This is very tricky, read this carefully.

What cucm does at this point will be construct the Geolocation string for both sides, and look for a Policy Relationship to take a decision, if is not found any Relationship Policy it will use teh default Policy

1. CUCM will start with Side B ⇒ Border,country=CO
2. Then with Side A ⇒ Interior: country=CO,A1=Guajira,A3=Maicao
3. It will look for a Relationship Policy Between Border:country=CO && Interior,country=CO,A1=Guajira,A3=Maicao

[+] CUCM will start constructing for 1 above

``` c hl:traversedStr
00005043.025 |16:25:42.255 |AppInfo  |LogicalPolicyTree -searchSourceTree, countFound[1], traversedStr[Border:country=VE:]
```

But there's no exist any relationship policy between Border:country=VE: so it will stop and use default poliicy

```c hl:findLogicalPartitionPolicyUsingVals
00005043.034 |16:25:42.255 |AppInfo  |LPPolicyManager -findLogicalPartitionPolicyUsingVals, DEFAULT POLICY found is [1]
00005043.035 |16:25:42.255 |AppInfo  |LPPolicyManager -findLogicalPartitionPolicyUsingVals, POLICY found is [7]
```

[+] As Call is allow, CUCM will strat constructing Geolation Info for calling in order to send the new invite with the info of Calling

```c
00005050.026 |16:25:42.257 |AppInfo  |GeolocInfo -buildGeolocationPidf, Pidf
```

[+] It will finish:

```c
00005050.027 |16:25:42.257 |AppInfo  |SIPGeoloc(0) - setOutgoingGeolocationInfo: Build result[4]
```

[+] And Invite is send with Geolocation Info:

``` c hl:":location"
00005057.001 |16:25:42.257 |AppInfo  |SIPTcp - wait_SdlSPISignal: Outgoing SIP TCP message to 10.207.166.104 on port 5060 index 8
[122,NET]
INVITE sip:3000@10.207.166.104:5060 SIP/2.0
Via: SIP/2.0/TCP 10.207.166.103:5060;branch=z9hG4bK1964b13f05
From: "Maicao Line 1" <sip:1001@10.207.166.103>;tag=55~6f9c10b0-08fe-4670-8b1e-a51c1b40ef5a-26044484
To: <sip:3000@10.207.166.104>
Date: Thu, 12 Sep 2024 21:25:42 GMT
Call-ID: 8f21d480-6e315c56-19-67a6cf0a@10.207.166.103
Supported: timer,resource-priority,replaces
Min-SE:  1800
User-Agent: Cisco-CUCM11.5
Allow: INVITE, OPTIONS, INFO, BYE, CANCEL, ACK, PRACK, UPDATE, REFER, SUBSCRIBE, NOTIFY
CSeq: 101 INVITE
Expires: 180
Allow-Events: presence, kpml
Supported: X-cisco-srtp-fallback,X-cisco-original-called
Call-Info: <sip:10.207.166.103:5060>;method="NOTIFY;Event=telephone-event;Duration=500"
Call-Info: <urn:x-cisco-remotecc:callinfo>;x-cisco-video-traffic-class=DESKTOP
Session-ID: 8e677fcde88ae06fd8ee10d584e9aa54;remote=00000000000000000000000000000000
Cisco-Guid: 2401358976-0000065536-0000000002-1738985226
Session-Expires:  1800
Geolocation: <cid:1001@10.207.166.103>;inserted-by="10.207.166.103"
P-Asserted-Identity: "Maicao Line 1" <sip:1001@10.207.166.103>
Remote-Party-ID: "Maicao Line 1" <sip:1001@10.207.166.103>;party=calling;screen=yes;privacy=off
Contact: <sip:1001@10.207.166.103:5060;transport=tcp>
Max-Forwards: 69
Content-Type: application/pidf+xml
Content-ID: 1001@10.207.166.103
Content-Length: 1065

<?xml version="1.0" encoding="UTF-8"?>
<presence xmlns="urn:ietf:params:xml:ns:pidf"
xmlns:gp="urn:ietf:params:xml:ns:pidf:geopriv10"
xmlns:cl=" urn:ietf:params:xml:ns:pidf:geopriv10:civicLoc"
xmlns:dm="urn:ietf:params:xml:ns:pidf:data-model"
xmlns:caps="urn:ietf:params:xml:ns:pidf:caps"
xmlns:cisco="http://www.cisco.com"
entity="pres:geotarget@example.com">
<dm:device id="sg89ae">
<caps:devcaps>
<cisco:gateway>false</cisco:gateway>
</caps:devcaps>
<gp:geopriv>
<gp:location-info>
<cl:civicAddress>
<cl:country>CO</cl:country>
<cl:A1>Guajira</cl:A1>
<cl:A3>Maicao</cl:A3>
<cl:A5>Finca Paz</cl:A5>
<cl:A6>5</cl:A6>
<cl:POD>Calle</cl:POD>
<cl:STS>84</cl:STS>
<cl:HNO>1</cl:HNO>
<cl:LOC>Lote 12</cl:LOC>
<cl:FLR>1</cl:FLR>
<cl:NAM>Carlos&apos;s House</cl:NAM>
<cl:PC>040021</cl:PC>
</cl:civicAddress>
</gp:location-info>
<gp:usage-rules>
<gp:retransmission-allowed>yes</gp:retransmission-allowed>
<gp:retention-expiry>2024-09-13T21:25:42Z</gp:retention-expiry>
</gp:usage-rules>
</gp:geopriv>
<timestamp>2024-09-12T21:25:42Z</timestamp>
</dm:device>
</presence>
```

[+] So, we should make the Geolocation Policy on the Other Sub, which makes sense, as we do not have any information about the location where i'm sending the call.

[+] Anyway, if i create a Policy like this

![[Pasted image 20240912172312.png]]

[+] And add the following Relationship Policy

![[Pasted image 20240912172337.png]]

[+] Calls through that Trunk stop working, either Marcaibo or Caracas

![[Pasted image 20240912172600.png]]

![[Pasted image 20240912172655.png]]

[+] The only difference on the logs is that after looking for the Geolocation String of the Trunk, it will look for the Calling:

```c hl:traverseStr,pkid
00017825.031 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, ptrSourceLastFound[0xf57243a8], devTYpe[Interior]
00017825.032 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, ptrSourceLastFound[0xf57243a8]
00017825.033 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, name == devType, ptrCurrentRoot[0xf5724478]
00017825.034 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, ELSE
00017825.035 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, while iterA, iterAName[country], iterAVal[CO]
00017825.036 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, while ptrCurrentRoot[0xf5724478]
00017825.037 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, iter.name[country], iter.val[CO]
00017825.038 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, Nam/Val found in tree, ptrCurrentRoot[0xf57244d8], targetCountFound[2]
00017825.039 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, while iterA, iterAName[A1], iterAVal[Guajira]
00017825.040 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, while ptrCurrentRoot[0xf57244d8]
00017825.041 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, iter.name[A1], iter.val[Guajira]
00017825.042 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, Nam/Val found in tree, ptrCurrentRoot[0xf5724538], targetCountFound[3]
00017825.043 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, while iterA, iterAName[A3], iterAVal[Maicao]
00017825.044 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, while ptrCurrentRoot[0xf5724538]
00017825.045 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, iter.name[A3], iter.val[Maicao]
00017825.046 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, Nam/Val found in tree, ptrCurrentRoot[0xf5724868], targetCountFound[4]
00017825.047 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, targetCountFound[4], ptrTargetLastFound[0xf5724868], traverseStr[Interior:country=CO,A1=Guajira,A3=Maicao,]

00017825.048 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchTargetTree, valid policy found[2], pkid[022d1dd2-827a-e852-f2cd-9d4a06a2c9ef], count[1]

00017825.049 |17:29:45.219 |AppInfo  |LogicalPolicyTree -searchPolicy, retVal[1]
00017825.050 |17:29:45.219 |AppInfo  |LPPolicyManager -findLogicalPartitionPolicyUsingVals, POLICY found is [9]
```

[+] now there is a policy for Border:country=CO && Interior,country=CO,A1=Guajira,A3=Maicao and is deny, check that it mention the policy found and its pkid, we can consult the rule with:

``` sql hl:glpm.pkid
run sql select glpa.name as GL_policy_a, tgla.name as device_type_a, glpb.name as GL_policy_b, tglb.name as device_type_b, tplpp.name as action from geolocationpolicymatrix glpm left join geolocationpolicy glpa on glpm.fkgeolocationpolicy_a = glpa.pkid left join geolocationpolicy glpb on glpm.fkgeolocationpolicy_b = glpb.pkid left join typegeolocationdevice tgla on glpm.tkgeolocationdevice_a = tgla.enum left join typegeolocationdevice tglb on glpm.tkgeolocationdevice_b = tglb.enum left join typelogicalpartitionpolicy tplpp on tplpp.enum = glpm.tklogicalpartitionpolicy where glpm.pkid = "022d1dd2-827a-e852-f2cd-9d4a06a2c9ef"

gl_policy_a   device_type_a gl_policy_b  device_type_b action
============= ============= ============ ============= ======
DefaultPolicy Border        MaicaoPolicy Interior      Deny
```

[+] From Cisco Official Documentation

https://www.cisco.com/c/en/us/support/docs/unified-communications/unified-communications-manager-callmanager/211280-Configuration-and-Troubleshoot-of-Logica.html

## **Points to ponder  

- Media Devices i.e (Media Termination Point) MTP, (Conference Bridge) CFB, Annunciator, (Music on Hold) MoH are not required to be associated with geolocation values.
- **There is no LP Policy check for VoIP to VoIP device call or feature with only VoIP participants. In other words, Interior to Interior policy is always allowed.**
- LPPolicyManager is a singleton process which interfaces with InMemDB and maintains policies in call processing as LP Policy Tree. During CUCM service startup, the LPPolicyManager reads the policies from InMemDB tables and constructs the LP Policy Tree. The Add/Delete/Update of a Policy in DB results in Change Notification to LPPolicyManager and change is affected in LP Policy Tree.

[+] Referencing to our table, it is important to mention that all calls between interior (Phones) devices even that they are on different sites, will be always allow as Logical Policies won't be checked, even if we have policies from Interior to Interior. So Policies will be either Interior to Border or Border to Interior.

``` 
╔════════════════╤══════════════╤════════╤═════════╤═══════════╗
║ Calling/Called │ Barranquilla │ Maicao │ Caracas │ Maracaibo ║
╠════════════════╪══════════════╪════════╪═════════╪═══════════╣
║ Barranquilla   │              │        │ Allow   │ Allow     ║
╟────────────────┼──────────────┼────────┼─────────┼───────────╢
║ Maicao         │              │        │ Allow   │ Deny      ║
╟────────────────┼──────────────┼────────┼─────────┼───────────╢
║ Caracas        │ Allow        │ Deny   │         │           ║
╟────────────────┼──────────────┼────────┼─────────┼───────────╢
║ Maracaibo      │ Deny         │ Deny   │         │           ║
╚════════════════╧══════════════╧════════╧═════════╧═══════════╝
```

[+] Now, it will be a task for the reader to construct the policies and make work the policies from the above table. For questions and suggestions you send an email to: aubarnes@cisco.com

Author: Angel Ubarnes (aubarnes)
18/Sept/2024