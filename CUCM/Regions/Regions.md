Author: Angel Ubarnes (aubarnes@cisco.com)

To match the codec for a call:
1. Unified Communications Manager takes the matching codecs from both sides of a call leg, 
2. filters out the codecs that exceed the configured maximum audio bit rate, 
3. and then picks the preferred codec among the codecs that are remaining in the list.

Supported Audio Codecs on CUCM:

| Audio Codec                           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| G.711                                 | The most commonly supported codec, used over the public switched telephone network.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| G.722                                 | Wideband codec often used in video conferences. This is always preferred by Unified Communications Manager over G.711, unless G.722 is disabled.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| G.722.1                               | Low complexity wideband codec operating at 24 and 32 kb/s. The audio quality approaches that of G.722 while using, at most, half the bit rate.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| G.728                                 | Low bit rate codec that video endpoints support.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| G.729                                 | Low bit rate codec with 8 kb/s compression that is supported by Cisco IP Phone 7900, and typically used for calls across a WAN link.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| GSM                                   | The global system for mobile communications (GSM) codec. GSM enables the MNET system for GSM wireless handsets to operate with Unified Communications Manager.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| L16                                   | Advanced Audio Coding-Low Delay (AAC-LD) is a super-wideband audio codec that provides superior sound quality for voice and music. This codec provides equal or improved sound quality over older codecs, even at lower bit rates.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| AAC-LD (mpeg4-generic)                | Supported for SIP devices, in particular, Cisco TelePresence systems.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| AAC-LD (MP4A-LATM)                    | Low-overhead MPEG-4 Audio Transport Multiplex (LATM) is a super-wideband audio codec that provides superior sound. Supported for SIP devices including Tandberg and some third-party endpoints.<br><br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Internet Speech Audio Codec (iSAC)    | An adaptive wideband audio codec, specially designed to deliver wideband sound quality with low delay in both low and medium bit rate applications.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Internet Low Bit Rate Codec (iLBC)    | Provides audio quality between G.711 and G.729 at bit rates of 15.2 and 13.3 kb/s while allowing for graceful speech quality degradation in a lossy network due to independently encoded speech frames. iLBC is supported for SIP, SCCP, H323, and MGCP devices.<br><br>                                                                                                                                                                                                                                                                                                                                                                                                                |
| Adaptive Multi-Rate (AMR)             | The required standard codec for 2.5G/3G wireless networks based on GSM (WDMA, EDGE, GPRS). This codec encodes narrowband (200-3400 Hz) signals at variable bit rates ranging from 4.75 to 12.2 kb/s with toll quality speech starting at 7.4 kb/s. AMR is supported only for SIP devices.                                                                                                                                                                                                                                                                                                                                                                                               |
| Adaptive Multi-Rate Wideband (AMR-WB) | Codified as G.722.2, an ITU-T standard speech codec formally known as Wideband, codes speech at about 16 kb/s. This codec is preferred over other narrowband speech codecs such as AMR and G.711 because it provides better speech quality due to a wider speech bandwidth of 50 Hz to 7000 Hz. AMR-WB is supported only for SIP devices.                                                                                                                                                                                                                                                                                                                                               |
| Opus                                  | Opus codec is an interactive speech and audio codec, specially designed to handle a wide range of interactive audio applications such as voice over IP, video conferencing, in-game chat, and live distributed music performance.<br><br>This codec scales from narrowband low bit rate to a very high quality bit rate ranging from 6 to 510 kb/s.<br><br>Opus codec support is enabled by default for all SIP devices. You can reconfigure Opus support via the Opus Codec Enabled service parameter (the default setting is Enabled for All Devices ). You can reconfigure this parameter to disable Opus codec support, or to enable support in non-recording devices only.<br><br> |
The supported codec by CUCM can be checked by:

``` sql
admin:run sql select * from typecodec

enum name                       moniker                          tkmedia minimumbandwidth defaultorder
==== ========================== ================================ ======= ================ ============
2    G.711 A-Law 64k            CODEC_G711_A_LAW_64k             1       64               18
3    G.711 A-Law 56k            CODEC_G711_A_LAW_56k             1       56               20
4    G.711 U-Law 64k            CODEC_G711_U_LAW_64k             1       64               17
5    G.711 U-Law 56k            CODEC_G711_U_LAW_56k             1       56               19
6    G.722 64k                  CODEC_G722_64k                   1       64               8
7    G.722 56k                  CODEC_G722_56k                   1       56               13
8    G.722 48k                  CODEC_G722_48k                   1       48               15
9    G.723.1 7k                 CODEC_G723_1_7k                  1       7                31
10   G.728 16k                  CODEC_G728_16k                   1       16               22
11   G.729 8k                   CODEC_G729_8k                    1       8                28
12   G.729a 8k                  CODEC_G729a_8k                   1       8                29
15   G.729b 8k                  CODEC_G729b_8k                   1       8                26
16   G.729ab 8k                 CODEC_G729ab_8k                  1       8                27
18   GSM Full Rate 13k          CODEC_GSM_FULL_RATE_13k          1       13               25
19   GSM Half Rate 6k           CODEC_GSM_HALF_RATE_6k           1       6                30
20   GSM Enhanced Full Rate 13k CODEC_GSM_ENHANCED_FULL_RATE_13k 1       13               24
25   L16 256k                   CODEC_L16_256k                   1       256              5
40   G.722.1 32k                CODEC_G722_1_32k                 1       32               12
41   G.722.1 24k                CODEC_G722_1_24k                 1       24               14
42   AAC-LD (MP4A Generic)      CODEC_LOSS_AAC_LD                1       256              2
43   MP4A-LATM 128k             CODEC_MP4A_LATM128k              1       128              1
44   MP4A-LATM 64k              CODEC_MP4A_LATM_64k              1       64               3
45   MP4A-LATM 56k              CODEC_MP4A_LATM_56k              1       56               4
46   MP4A-LATM 48k              CODEC_MP4A_LATM_48k              1       48               6
47   MP4A-LATM 32k              CODEC_MP4A_LATM_32k              1       32               10
48   MP4A-LATM 24k              CODEC_MP4A_LATM_24k              1       24               16
86   ILBC 16k                   CODEC_ILBC_16k                   1       16               21
89   ISAC 32k                   CODEC_ISAC_32k                   1       32               9
97   AMR (5k-13k)               CODEC_AMR                        1       5                23
98   AMR-WB (7k-24k)            CODEC_AMR_WB                     1       7                11
90   OPUS (6k-510k)             CODEC_OPUS   
```


Let's suppose, we have a call from Phone A (blue one) to Phone B (yellow one)

![[Pasted image 20240918173813.png]]

![[Pasted image 20240918101535.png]]
Phone A, Codecs Supported:

```
╔═══════════╤════════╤════════╤════════╤═════════╤═══════╤══════╗
║ Codec     │ G.711a │ G.711µ │ G.729  │ G.729ab │ G.728 │ iLBC ║
╠═══════════╪════════╪════════╪════════╪═════════╪═══════╪══════╣
║ BandWidth │ 64     │ 64     │ 8      │ 64      │ 16    │ 16   ║
╚═══════════╧════════╧════════╧════════╧═════════╧═══════╧══════╝
```

![[Pasted image 20240918101619.png]]
Phone B, Codecs Supported:

```
╔═══════════╤════════╤═══════╤═══════╤═══════╤═══════╤═══════╗
║ Codec     │ G.711a │ iLBC  │ G.729 │ G.726 │ G.722 │ G.728 ║
╠═══════════╪════════╪═══════╪═══════╪═══════╪═══════╪═══════╣
║ BandWidth │ 64     │ 16    │ 8     │ 24    │ 64    │ 16    ║
╚═══════════╧════════╧═══════╧═══════╧═══════╧═══════╧═══════╝
```

On CUCM
![[Pasted image 20240918101709.png]]

If MaxBitRate configured between the Region of Phone A and Phone B is configured to 32kbps, it will filter out:

```
╔════════╤═══════╤══════╤═══════╤═══════╗
║ Codec  │ G.728 │ iLBC │ G.726 │ G.729 ║
╠════════╪═══════╪══════╪═══════╪═══════╣
║ PhoneA │ 16    │ 16   │       │ 8     ║
╟────────┼───────┼──────┼───────┼───────╢
║ PhoneB │ 16    │ 16   │ 24    │ 8     ║
╚════════╧═══════╧══════╧═══════╧═══════╝
```

Then, CUCM will remove the codecs not supported for both parties ⇒ { G.728, iLBC, G.729 }

```
╔════════╤═══════╤══════╤═══════╗
║ Codec  │ G.728 │ iLBC │ G.729 ║
╠════════╪═══════╪══════╪═══════╣
║ PhoneA │ 16    │ 16   │ 8     ║
╟────────┼───────┼──────┼───────╢
║ PhoneB │ 16    │ 16   │ 8     ║
╚════════╧═══════╧══════╧═══════╝
```

Then, CUCM will select one codec by matching that above list with ⇒ { G.728, iLBC, G.729 } by checking the CodecList configured for the region relationship between Regions between phones. In this case Factory Default lossy.


![[Pasted image 20240918173813.png]]

| Region                           | CodecList             | Audio_BW | Video BW | Immersive |
| -------------------------------- | --------------------- | -------- | -------- | --------- |
| Region_A_Test With Region_B_Test | Factory Default lossy | 32       | 18       | 6         |

[+] Checking the content of the codec prerence list, CUCM will look top to down until matches on of the codec inside ⇒  { G.728, iLBC, G.729 }, the first one matched is ILBC, so, for that Call We will use ILBC:

``` sql hl:"23,4"
run sql select cl.name as codec_preference_list, cl.description,c.name as codec,clm.preferenceorder as preference,c.minimumbandwidth as kbps,c.enum as payload from codeclistmember as clm \
inner join codeclist as cl on clm.fkcodeclist = cl.pkid \
inner join typecodec as c on c.enum = clm.tkcodec \
where cl.name  = 'Factory Default lossy' \
order by cl.name,clm.preferenceorder,kbps ASC


codec_preference_list codec                      preference kbps payload
===================== ========================== ========== ==== =====
Factory Default lossy OPUS (6k-510k)             1          6    90
Factory Default lossy MP4A-LATM 56k              4          56   45
Factory Default lossy MP4A-LATM 48k              6          48   46
Factory Default lossy ISAC 32k                   8          32   89
Factory Default lossy AMR-WB (7k-24k)            9          7    98
Factory Default lossy MP4A-LATM 32k              10         32   47
Factory Default lossy G.722.1 32k                12         32   40
Factory Default lossy G.722 56k                  13         56   7
Factory Default lossy G.722.1 24k                14         24   41
Factory Default lossy G.722 48k                  15         48   8
Factory Default lossy MP4A-LATM 24k              16         24   48
Factory Default lossy G.711 U-Law 56k            19         56   5
Factory Default lossy G.711 A-Law 56k            20         56   3
Factory Default lossy ILBC 16k                   21         16   86
Factory Default lossy G.728 16k                  22         16   10
Factory Default lossy AMR (5k-13k)               23         5    97
Factory Default lossy GSM Enhanced Full Rate 13k 24         13   20
Factory Default lossy GSM Full Rate 13k          25         13   18
Factory Default lossy G.729b 8k                  26         8    15
Factory Default lossy G.729ab 8k                 27         8    16
Factory Default lossy G.729 8k                   28         8    11
Factory Default lossy G.729a 8k                  29         8    12
Factory Default lossy GSM Half Rate 6k           30         6    19
Factory Default lossy G.723.1 7k                 31         7    9
```


 =====================
``` hl:"g.729"
╔════════╤═══════╤══════╤═══════╤═══════╗
║ Codec  │ G.728 │ iLBC │ G.726 │ G.729 ║
╠════════╪═══════╪══════╪═══════╪═══════╣
║ PhoneA │ 16    │ 32   │       │ 8     ║
╟────────┼───────┼──────┼───────┼───────╢
║ PhoneB │ 16    │ 32   │ 24    │ 8     ║
╚════════╧═══════╧══════╧═══════╧═══════╝
```

[+] We can check the region relationship

``` sql
run sql select s1.name as Region_A, s2.name as Region_B, cdl.name as Codec_list,rgm.audiobandwidth as Audio_BW, rgm.videobandwidth as Video_BW, rgm.immersivebandwidth as Immersive_BW from regionmatrix as rgm left join region as s2 on rgm.fkregion_b = s2.pkid left join region as s1 on rgm.fkregion_a = s1.pkid left join codeclist as cdl on cdl.pkid = rgm.fkcodeclist order by audio_bw ASC

region_a      region_b      codec_list                           audio_bw video_bw immersive_bw
============= ============= ==================================== ======== ======== ============
AngelTest     AngelTest     Factory Default lossy                8        64       16
dCloud_Region dCloud_Region Factory Default low loss - No G722.1 64       64       64
AngelTest     dCloud_Region Factory Default low loss             64       16       8

```




============================ Recreation  ===============================

1. Create CM Group: CM_Group_Test and Assign the publisher to this group

![[Pasted image 20240918174951.png]]

```sql hl:3,8,CM_group_test
run sql insert into callmanagergroup (name) values ('CM_group_test')

^ Adding 'CM_group_test' as CallManager Group 


run sql insert into callmanagergroupmember (fkcallmanagergroup,fkcallmanager,priority) values ((select pkid from callmanagergroup where name like 'CM_group_test'),(select pkid from callmanager where ctiid = 1),1)

^Assign Pub to that CM Group

```

2. Create a DateTimeGroup with default settings (this is just a required field when creating a DevicePool)

``` sql
run sql insert into datetimesetting (name) values ('DTG_test')
```

![[Pasted image 20240918175535.png]]

3. Create 2 Regions

```sql
run sql insert into region (name) values ('Region_A_Test')

run sql insert into region (name) values ('Region_B_Test')
```

![[Pasted image 20240918175545.png]]


4. Create two Device Pool with Regions, CM Group and Date Time Group

``` sql hl:DTG_test,Region_A_Test,CM_group_test,Region_B_Test,DevicePool_A_Test,DevicePool_B_Test
run sql insert into devicepool (fkcallmanagergroup,fkdatetimesetting,fkregion,name) values ((select pkid from callmanagergroup where name like 'CM_group_test'),(select pkid from datetimesetting where name like 'DTG_test'),(select pkid from region where name like 'Region_A_Test'),'DevicePool_A_Test')

run sql insert into devicepool (fkcallmanagergroup,fkdatetimesetting,fkregion,name) values ((select pkid from callmanagergroup where name like 'CM_group_test'),(select pkid from datetimesetting where name like 'DTG_test'),(select pkid from region where name like 'Region_B_Test'),'DevicePool_B_Test')
```

![[Pasted image 20240918175922.png]]


5. Create two devices and assign to different regions, and make sure they are able to call between them.

![[Pasted image 20240918180142.png]]Codecs for IP Communicator: [https://www.cisco.com/c/en/us/products/collateral/collaboration-endpoints/ip-communicator/data_sheet_c78-669663.html](https://www.cisco.com/c/en/us/products/collateral/collaboration-endpoints/ip-communicator/data_sheet_c78-669663.html)
Codecs for Jabber Windows: [https://community.cisco.com/t5/collaboration-applications/jabber-for-windows-codec-support/td-p/3031937](https://community.cisco.com/t5/collaboration-applications/jabber-for-windows-codec-support/td-p/3031937)>
```
╔══════════════════════════════════════════════════════╤════════════════════╗
║ Codec Ip Communicatior                               │ Codec Jabber       ║
╠══════════════════════════════════════════════════════╪════════════════════╣
║ G.722 wideband,                                      │ G.711 A-law        ║
╟──────────────────────────────────────────────────────┼────────────────────╢
║ G.711a,                                              │ G.711 µ-law/Mu-law ║
╟──────────────────────────────────────────────────────┼────────────────────╢
║ G711ų,                                               │ G.722              ║
╟──────────────────────────────────────────────────────┼────────────────────╢
║ iLBCm,                                               │ G.722.1            ║
╟──────────────────────────────────────────────────────┼────────────────────╢
║ G.729a,                                              │ G.729              ║
╟──────────────────────────────────────────────────────┼────────────────────╢
║ G.729ab,                                             │ G.729a             ║
╟──────────────────────────────────────────────────────┼────────────────────╢
║ G.729b (Skinny Client Control Protocol [SCCP] only), │ Opus               ║
╟──────────────────────────────────────────────────────┼────────────────────╢
║ Internet Speech Audio Codec (iSAC)                   │                    ║
╚══════════════════════════════════════════════════════╧════════════════════╝
```

6. We are gonna try to use different codecs based on regions:

```
╔═══════════════╤═══════════════╤═══════════════════════╤═════════════════╗
║ Region A      │ Region B      │ CodecList             │ Codec           ║
╠═══════════════╪═══════════════╪═══════════════════════╪═════════════════╣
║ Region_A_Test │ Region_B_Test │ Codec list g729a      │ G.729a 8k       ║
╟───────────────┼───────────────┼───────────────────────┼─────────────────╢
║ Region_A_Test │ Region_A_Test │ Codec list g729 a-law │ G.711 A-Law 64k ║
╟───────────────┼───────────────┼───────────────────────┼─────────────────╢
║ Region_B_Test │ Region_B_Test │ Codec list g729 u-law │ G.711 U-Law 64k ║
╚═══════════════╧═══════════════╧═══════════════════════╧═════════════════╝
```

![[Pasted image 20240918181257.png]]

![[Pasted image 20240918181352.png]]

![[Pasted image 20240918181811.png]]

7.  Create 3 Audio Codec Preference List

```sql
run sql insert into codeclist (name,description,isstandard) values ('Codec list g729a','Testing purposes for g729a','f')

run sql insert into codeclistmember (fkcodeclist,tkcodec,preferenceorder) values ((select pkid from codeclist where name like 'Codec list g729a'),(select enum from typecodec where name like 'G.729a 8k'),1)
```

```sql
run sql insert into codeclist (name,description,isstandard) values ('Codec list g729 a-law','Testing purposes for g729alaw','f')

run sql insert into codeclistmember (fkcodeclist,tkcodec,preferenceorder) values ((select pkid from codeclist where name like 'Codec list g729 a-law'),(select enum from typecodec where name like 'G.711 A-Law 64k'),1)
```

```sql
run sql insert into codeclist (name,description,isstandard) values ('Codec list g729 u-law','Testing purposes for g729Ulaw','f')

run sql insert into codeclistmember (fkcodeclist,tkcodec,preferenceorder) values ((select pkid from codeclist where name like 'Codec list g729 u-law'),(select enum from typecodec where name like 'G.711 U-Law 64k'),1)
```

8. Creating the regions relationships and assign it the preferred codeclist

``` sql
run sql insert into regionmatrix (fkregion_a,fkregion_b,videobandwidth,fkcodeclist,immersivebandwidth,audiobandwidth) values ((select pkid from region where name like 'Region_A_Test'),(select pkid from region where name like 'Region_B_Test'),6000,(select pkid from codeclist where name like 'Codec list g729a'),2147483647,8)

run sql insert into regionmatrix (fkregion_a,fkregion_b,videobandwidth,fkcodeclist,immersivebandwidth,audiobandwidth) values ((select pkid from region where name like 'Region_A_Test'),(select pkid from region where name like 'Region_A_Test'),6000,(select pkid from codeclist where name like 'Codec list g729 a-law'),2147483647,64)

run sql insert into regionmatrix (fkregion_a,fkregion_b,videobandwidth,fkcodeclist,immersivebandwidth,audiobandwidth) values ((select pkid from region where name like 'Region_B_Test'),(select pkid from region where name like 'Region_B_Test'),6000,(select pkid from codeclist where name like 'Codec list g729 u-law'),2147483647,64)
```

9. Checking The relationships
``` sql
run sql select s1.name as Region_A, s2.name as Region_B, cdl.name as Codec_list,rgm.audiobandwidth as Audio_BW, rgm.videobandwidth as Video_BW, rgm.immersivebandwidth as Immersive_BW from regionmatrix as rgm left join region as s2 on rgm.fkregion_b = s2.pkid left join region as s1 on rgm.fkregion_a = s1.pkid left join codeclist as cdl on cdl.pkid = rgm.fkcodeclist order by audio_bw ASC

region_a      region_b      codec_list                           audio_bw video_bw immersive_bw
============= ============= ==================================== ======== ======== ============
Region_A_Test Region_B_Test Codec list g729a                     8        6000     2147483647
Region_A_Test Region_A_Test Codec list g729 a-law                64       6000     2147483647
Region_B_Test Region_B_Test Codec list g729 u-law                64       6000     2147483647
```

10. We do Receive the supported codecs for the device on the SDP content Either on Invite (early offer) or 200ok (delay offer)

[+] SDP IP Communicator

``` C
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:18 G729/8000
a=rtpmap:9 G722/8000
a=rtpmap:116 iLBC/8000
a=rtpmap:124 ISAC/16000
a=rtpmap:101 telephone-event/8000
```

[+] SDP Jabber

```c
a=rtpmap:114 opus/48000/2
a=rtpmap:9 G722/8000
a=rtpmap:104 G7221/16000
a=rtpmap:105 G7221/16000
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:101 telephone-event/8000
a=rtpmap:100 H264/90000
a=rtpmap:98 H264/90000
a=rtpmap:126 H264/90000
a=rtpmap:97 H264/90000
a=rtpmap:111 x-ulpfecuc/8000
a=rtpmap:98 H264/90000
a=rtpmap:126 H264/90000
a=rtpmap:97 H264/90000
a=rtpmap:111 x-ulpfecuc/8000
a=rtpmap:125 H224/4800
```

NOTE:

The payload (number identififer for a Codec) for the codecs that SIP protocol uses is different that the codec payload that CUCM handle internally, from the below table we can know what are they on CUCM.

```sql
admin:run sql select enum,name from typecodec
enum name
==== ==========================
2    G.711 A-Law 64k
3    G.711 A-Law 56k
4    G.711 U-Law 64k
5    G.711 U-Law 56k
6    G.722 64k
7    G.722 56k
8    G.722 48k
9    G.723.1 7k
10   G.728 16k
11   G.729 8k
12   G.729a 8k
15   G.729b 8k
16   G.729ab 8k
18   GSM Full Rate 13k
19   GSM Half Rate 6k
20   GSM Enhanced Full Rate 13k
25   L16 256k
40   G.722.1 32k
41   G.722.1 24k
42   AAC-LD (MP4A Generic)
43   MP4A-LATM 128k
44   MP4A-LATM 64k
45   MP4A-LATM 56k
46   MP4A-LATM 48k
47   MP4A-LATM 32k
48   MP4A-LATM 24k
86   ILBC 16k
89   ISAC 32k
97   AMR (5k-13k)
98   AMR-WB (7k-24k)
90   OPUS (6k-510k)
```

If a device sends a codec on the SDP message and this is not handle by CUCM it will be ignored.

For cheking the payloads for SIP, refer to:
[https://www.iana.org/assignments/rtp-parameters/rtp-parameters.xhtml](https://www.iana.org/assignments/rtp-parameters/rtp-parameters.xhtml)

[+] From CUCM Traces we can get the region relationship for two legs

``` c hl:"MediaConnectRequest,MediaCoordinator,audioCapCount,region"
10287403.000 |09:18:24.452 |SdlSig   |MediaConnectRequest                    |wait                           |MediaCoordinator(1,100,118,1)    |ConnectionManager(1,100,44,1)    |1,100,251,4851.2304^198.18.1.36^*        |[R:N-H:0,N:3,L:0,V:0,Z:0,D:0] 
Party1: MR=0 
CI=30514126 audioCapCount=7 region=Region_A_Test xferMode=16 mrid=0 audioId=0 MMCap=0x1 sipConfig: BFCPAllowed=F IXAllowed=F activeCap=0 cryptoCapCount=0 flushIns=0 dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F IFPid=(0,0,0,0) dtMedia=F honorCodec=F EOType=0  DTMF Caps(1,3,(101:8000,),0,F) confID=0 connType=3 connStatus=0 mtpPre=F teleEve=0 IFCreated=F IFHandling=0 FS=0 mcNodeId=0LatentCaps=null dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F 
Party2: MR=0 
CI=30514127 audioCapCount=7 region=Region_B_Test xferMode=16 mrid=0 audioId=0 MMCap=0x3f sipConfig: BFCPAllowed=T IXAllowed=T activeCap=0 cryptoCapCount=0 flushIns=0 dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F IFPid=(0,0,0,0) dtMedia=F honorCodec=F EOType=0  DTMF Caps(1,3,(101:8000,),0,F) confID=0 connType=3 connStatus=0 mtpPre=F teleEve=0 IFCreated=F IFHandling=0 FS=0 mcNodeId=0LatentCaps=null dtm.mode=0 dtm.CI=0 dtm.MTPForDTMF=F 
reConnType=0 videoCall=F AllowedCallType=0x0 mtpChanged=F precLvl=5 resCap=0 party1.mMediaCoordinatorNodeId=0 party2.mMediaCoordinatorNodeId=0 sideBAns= T
```

To look all this lines on a set of files, we can search for:

```c, hl:1|CI=\d+
\d+\.\d{3} \|\d+:\d+:\d+\.\d{3} \|SdlSig.+\|MediaConnectRequest.+\|MediaCoordinator.+\|ConnectionManager.+Party1\:.+CI=\d+ audioCapCount=\d+ region=.+
```

If we have the Party A CI, example for 30514126 :

```c, hl:1|30514126
\d+\.\d{3} \|\d+:\d+:\d+\.\d{3} \|SdlSig.+\|MediaConnectRequest.+\|MediaCoordinator.+\|ConnectionManager.+Party1\:.+CI=30514126 audioCapCount=\d+ region=.+
```

If we have the Party B CI, example for 30514127 :

```c, hl:1|30514127
\d+\.\d{3} \|\d+:\d+:\d+\.\d{3} \|SdlSig.+\|MediaConnectRequest.+\|MediaCoordinator.+\|ConnectionManager.+Party1\:.+CI=\d+ audioCapCount=\d+ region=.+Party2: MR=0 CI=30514127.+
```

MediaCoordinator will create MediaManager process

```c,hl:1|17
10287403.001 |09:18:24.452 |Created  |MediaManager(1,100,119,17)|MediaCoordinator(1,100,118,1)|NumOfCurrentInstances: 1
```

Then, MediaManager and its number of instace will show this message when we can see the codecs

```c,hl:1|(17)
10287413.006 |09:18:24.452 |AppInfo  |DET-MediaManager-(17)::preCheckCapabilities,
region1=Region_A_Test, region2=Region_B_Test,
Pty1 capCount=7 (Cap,ptime)= (4,20) (2,20) (11,20) (12,20) (6,20) (86,20) (89,30),
Pty2 capCount=7 (Cap,ptime)= (40,0) (41,0) (4,20) (2,20) (11,20) (12,20) (115,0)
```

| Trace   | Payload | Codec           | Advertised Codec on SIP Invite |
| ------- | ------- | --------------- | ------------------------------ |
| (4,20)  | 4       | G.711 U-Law 64k | a=rtpmap:0 PCMU/8000           |
| (2,20)  | 2       | G.711 A-Law 64k | a=rtpmap:8 PCMA/8000           |
| ==(11,20)== | ==11==      | ==G.729 8k==        | ==a=rtpmap:18 G729/8000==          |
| ==(12,20)== | ==12==      | ==G.729a 8k==       | ==a=fmtp:18 annexb=no==            |
| (6,20)  | 6       | G.722 64k       | a=rtpmap:9 G722/8000           |
| (86,20) | 86      | ILBC 16k        | a=rtpmap:116 iLBC/8000         |
| (89,30) | 89      | ISAC 32k        | a=rtpmap:124 ISAC/16000        |

| Trace    | Payload | Codec           | Advertised Codec on SIP Invite |
| -------- | ------- | --------------- | ------------------------------ |
| (40,0)   | 40      | G.722.1 32k     | a=rtpmap:105 G7221/16000       |
| (41,0)   | 41      | G.722.1 24k     | a=fmtp:105 bitrate=24000       |
| (4,20)   | 4       | G.711 U-Law 64k | a=rtpmap:0 PCMU/8000           |
| (2,20)   | 2       | G.711 A-Law 64k | a=rtpmap:8 PCMA/8000           |
| ==(11,20)==  | ==11==      | ==G.729 8k==        | ==a=rtpmap:18 G729/8000==          |
| ==(12,20)==  | ==12==      | ==G.729a 8k==       | ==a=fmtp:18 annexb=no==            |
| (115,30) | 115     | H264_FEC        | a=rtpmap:100 H264/90000        |
We will be using the the codec list Codec list g729a this inter region is set to 8kbps, and the preference codec is G.729a 8k with payload 12

``` hl:3
region_a      region_b      codec_list                           audio_bw video_bw immersive_bw
============= ============= ==================================== ======== ======== ============
Region_A_Test Region_B_Test Codec list g729a                     8        6000     2147483647
Region_A_Test Region_A_Test Codec list g729 a-law                64       6000     2147483647
Region_B_Test Region_B_Test Codec list g729 u-law                64       6000     2147483647
```

```hl:5
codec_preference_list                description                   codec                      preference kbps payload
==================================== ============================= ========================== ========== ==== =======
Codec list g729 a-law                Testing purposes for g729alaw G.711 A-Law 64k            1          64   2
Codec list g729 u-law                Testing purposes for g729Ulaw G.711 U-Law 64k            1          64   4
Codec list g729a                     Testing purposes for g729a    G.729a 8k                  1          8    12
```

MediaManager creates MediaExchange

```c
10287413.014 |09:18:24.452 |Created|MediaExchange(1,100,114,20)      |MediaManager(1,100,119,17)|NumOfCurrentInstances: 1
```

Notice that this MediaExchange creates two instances of SipInterface and checks InterRegion bandwith

```c hl:"SIPInterface,AppInfo"

10287415.008 |09:18:24.453 
|AppInfo  |DET-SIPInterface-(0)- CI=30514126
mrId=0 PartySide=65 
HOP RegionBwKbps[ A=8 V = 6000 I = 2147483647 ]  E2E RegionBwKbps[ A=8 V = 6000 I = 2147483647 ]

vid=0 AllowedCallType=0x00000001 MCast=0 port=61385 IpAddrMode(my=0 peer=0 farEnd=0) XferMode(peer=16 farEnd=0) mediaReq=0 farEndMedReq=0 PassThru(A=2 V=2) Qos(0,1) vidPref(0) isPartyA(1) otherAgentPorts=0  FSInfo=0

10287415.009 |09:18:24.453 |Created|SIPInterface(1,100,186,4)        |MediaExchange(1,100,114,20)|NumOfCurrentInstances: 1

10287415.010 |09:18:24.453 |AppInfo  |DET-SIPInterface-(0)- CI=30514127 mrId=0 PartySide=66 HOP RegionBwKbps[ A=8 V = 6000 I = 2147483647 ]  E2E RegionBwKbps[ A=8 V = 6000 I = 2147483647 ]  vid=0 AllowedCallType=0x00000001 MCast=0 port=57855 IpAddrMode(my=0 peer=0 farEnd=0) XferMode(peer=16 farEnd=0) mediaReq=0 farEndMedReq=0 PassThru(A=2 V=2) Qos(0,1) vidPref(0) isPartyA(0) otherAgentPorts=0  FSInfo=0

10287415.011 |09:18:24.453 |Created|SIPInterface(1,100,186,5)        |MediaExchange(1,100,114,20)|NumOfCurrentInstances: 2
```

According to the region BW the codecs will be filtering out, we see 11 and 12, G.729 8k and G.729a 8k, respectively

```c
10287461.014 |09:18:24.456 |AppInfo  |DET-SIPInterface-(5)::filterAudioCaps, doSort=1 audioRegion=8 kbps, nAudio=1 lineIndex=0
10287461.015 |09:18:24.456 |AppInfo  |DET-SIPInterface-(5)::filterAudioCaps, mAudiomLine[0].capCount=7 index=0 after filter
10287461.016 |09:18:24.456 |AppInfo  |DET-MediaUtility-::getClockRateList using default clockrate 8k for codec 11
10287461.017 |09:18:24.456 |AppInfo  |DET-MediaUtility-::getClockRateList using default clockrate 8k for codec 12
```

Getting Codec from Preference List

``` c
10287461.021 |09:18:24.456 |AppInfo  |DET-MediaUtility-::getCodecPrefOption, xferModeA=16 xferModeB=16 honorOfferCodecPrefA=0 honorOfferCodecPrefB=0 PREF_LIST
10287461.023 |09:18:24.456 |AppInfo  |DET-RegionsServer::matchCapabilities-- savedOption=1, PREF_LIST,
regionA=Region_B_Test regionB=Region_A_Test
latentCaps(A=0, B=0) kbps=8, capACount=7, capBCount=2
```

Getting G.729a 8k from the Audio Codec Preference List

```c
10287461.054 |09:18:24.456 |AppInfo  |DET-RegionsServer::matchCapabilities-- savedOption=0, PREF_LIST, 
regionA=Region_B_Test regionB=Region_A_Test 
latentCaps(A=0, B=0) kbps=8, capACount=7, capBCount=2
10287461.055 |09:18:24.456 |AppInfo  |DET-SDPMsg-()::negotiateAudioCaps, audiomLine.caps[0].payloadCapability=12, maxFramesPerPacket=20,  dynamicPayload=0
10287461.064 |09:18:24.456 |AppInfo  |DET-MediaUtility-::calculateAudioRawBitRateKbpsForNonSimulcastCall() - audBitRateKbps= 8 audRegKbps = 8 bitRateDir = Transmit codec = 12
```

Then we see an SDP answer from the AudioInterface from the Call Leg B, in this case:  SIPInterface(1,100,186,5), we can see the codec and the port selectd, and the IP as well

``` c
10287469.000 |09:18:24.457 |SdlSig   |SDPAnswer|outCall_200Rcvd|SIPCdpc(1,100,180,73)|SIPInterface(1,100,186,5)|1,100,251,4851.2304^198.18.1.36^*|[R:N-H:0,N:6,L:0,V:0,Z:0,D:0] ] 
nAudio=1 stackIdx=1 audioCapCount=1 Caps[12(20)] port=18402 IP= ipAddrType=0 ipv4=10.16.169.162 
SDPMode=0 mediaAttr=0x0 SP=F RTP=T SRTP=F idle=F QoS=F enabledMask=0 rtcbFbCount=0LatentCaps=null TCL_UNSPECIFIED ptime=0 ~nVideo=2 stackIdx=2 
videoCapCount=4, 
[100 pt=31 h235=0],
[101 pt=34 h235=0 ver=0(H263VERSION_ORIG) bit_mask=0x0],
[101 pt=96 h235=0 ver=1(H263VERSION_1998) bit_mask=0x0],
[103 pt=97 h235=0] 
port=0 ipAddrType=0 ipv4=10.16.169.162 SDPMode=3 RTP=T SRTP=F idle=F mVideoContentId=1 mContentTypeCount=0enabledMask=0rtcbFbCount=0 TCL_UNSPECIFIED, stackIdx=3 
videoCapCount=4, 
[100 pt=31 h235=0],
[101 pt=34 h235=0 ver=0(H263VERSION_ORIG) bit_mask=0x0],
[101 pt=96 h235=0 ver=1(H263VERSION_1998) bit_mask=0x0],
[103 pt=97 h235=0] 
port=0 ipAddrType=0 ipv4=10.16.169.162 SDPMode=3 RTP=T SRTP=F idle=F mVideoContentId=2 mContentTypeCount=0enabledMask=0rtcbFbCount=0 TCL_UNSPECIFIED ~nApp=1 stackIdx=5 
CapCount=1,106 port=0 ipAddrType=0 ipv4=10.16.169.162 SDPMode=3 ~nT38Fax=0 ^nBFCPApp=1 stackIdx=4 port=0 ipAddrType=0 ipv4=0.0.0.0 flCtrlRoleMask=0 confID= userID= floorIdStrmAssoSize=0 ^nIXApp=1 stackIdx=6 port=0 ipAddrType=0 
ipv4=10.16.169.162 xmapListSize=0 RTP=T SRTP=F Transport=17 DTMFMethod=3 DTMFConfig=1 RFC2833PT=(101:8000,) wantDTMF=0 provideOOB=F 
keepAudiomLineForT38=F FaxInviteWithValidIP=F FCOffer=0 transID=0 mANATAddrPref=0V4 negIpAddrType=0V4 MTPAllocated=0 mAllowMultiCodecsInAnswer=F
```

For the other SipInterface

```c
10287475.008 |09:18:24.458 |AppInfo  |DET-SIPInterface-(4)::filterAudioCaps, doSort=1 audioRegion=8 kbps, nAudio=1 lineIndex=0
10287475.009 |09:18:24.458 |AppInfo  |DET-SIPInterface-(4)::filterAudioCaps, mAudiomLine[0].capCount=7 index=0 after filter
10287475.010 |09:18:24.458 |AppInfo  |DET-MediaUtility-::getClockRateList using default clockrate 8k for codec 11
10287475.011 |09:18:24.458 |AppInfo  |DET-MediaUtility-::getClockRateList using default clockrate 8k for codec 12

|10287475.045 \|09:18:24.458 \|AppInfo  \|DET-SDPMsg-()::negotiateAudioCaps, audiomLine.caps[0].payloadCapability=12, maxFramesPerPacket=20,  dynamicPayload=0<br><br>10287475.056 \|09:18:24.458 \|AppInfo  \|DET-MediaUtility-::calculateAudioRawBitRateKbpsForNonSimulcastCall() - audBitRateKbps= 8 audRegKbps = 8 bitRateDir = Transmit codec = 12|
```

```c
10287482.000 |09:18:24.459 |SdlSig   |SDPAnswer|inCall_delivered|SIPCdpc(1,100,180,72)|SIPInterface(1,100,186,4)|1,100,251,4851.2304^198.18.1.36^*|[R:N-H:0,N:8,L:0,V:0,Z:0,D:0]  Unrecognized attributes: mAttrListSize=2  a=cisco-mari:v1 a=cisco-mari-rate] 
nAudio=1 stackIdx=1 audioCapCount=1 Caps[12(20)] port=24632 IP= ipAddrType=0 ipv4=198.18.1.36 
SDPMode=0 mediaAttr=0x0 SP=F RTP=T SRTP=F idle=F QoS=F enabledMask=0 rtcbFbCount=0
LatentCaps=null TCL_UNSPECIFIED ptime=0 ~nVideo=0 ~nApp=0 ~nT38Fax=0 ^nBFCPApp=0 ^nIXApp=0 
DTMFMethod=3 DTMFConfig=1 RFC2833PT=(101:8000,) wantDTMF=0 provideOOB=F keepAudiomLineForT38=F F
axInviteWithValidIP=F FCOffer=0 transID=0 mANATAddrPref=0V4 negIpAddrType=0V4 MTPAllocated=0 mAllowMultiCodecsInAnswer=F
```

Finally we send and ACK to the Call Leg B, to send the port and the codec negotiated

```c hl:23,24,25
10287498.001 |09:18:24.462 |AppInfo  |SIPTcp - wait_SdlSPISignal: Outgoing SIP TCP message to 198.18.1.36 on port 61385 index 58492 
[3841494,NET]
ACK sip:d420d1d2-ad14-cf24-fad4-1f13f85bf4db@198.18.1.36:61385;transport=tcp SIP/2.0
Via: SIP/2.0/TCP 198.18.133.3:5060;branch=z9hG4bKa1c5a11e74631
From: <sip:1134@198.18.133.3>;tag=1228733~0a37f347-c14b-44f5-8299-186aa2addafc-30514127
To: <sip:6016@cucm1.dcloud.cisco.com>;tag=005056a9a57c3c02000013a4-0000512e
Date: Wed, 01 Feb 2023 15:18:21 GMT
Call-ID: a85d6200-10001-a1c4c-93f492a@198.18.133.3
User-Agent: Cisco-CUCM12.5
Max-Forwards: 70
CSeq: 101 ACK
Allow-Events: presence
Session-ID: 393bcdb429c149398852e75aa1228730;remote=00007a4000105000a000005056a9a57c
Content-Type: application/sdp
Content-Length: 694

v=0
o=CiscoSystemsCCM-SIP 1228733 1 IN IP4 198.18.133.3
s=SIP Call
c=IN IP4 10.16.169.162
b=AS:24
t=0 0
m=audio 18402 RTP/AVP 18 101
a=rtpmap:18 G729/8000
a=fmtp:18 annexb=no
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-15
m=video 0 RTP/AVP 31 34 96 97
a=rtpmap:31 H261/90000
a=rtpmap:34 H263/90000
a=rtpmap:96 H263-1998/90000
a=rtpmap:97 H264/90000
a=content:main
a=inactive
m=video 0 RTP/AVP 31 34 96 97
a=rtpmap:31 H261/90000
a=rtpmap:34 H263/90000
a=rtpmap:96 H263-1998/90000
a=rtpmap:97 H264/90000
a=content:slides
a=inactive
m=application 0 UDP/BFCP *
c=IN IP4 0.0.0.0
m=application 0 RTP/AVP 96
a=rtpmap:96 H224/0
a=inactive
m=application 0 UDP/UDT/IX *

```

```c hl:31,32,33,34
10287523.001 |09:18:24.466 |AppInfo  |SIPTcp - wait_SdlSPISignal: Outgoing SIP TCP message to 10.16.169.162 on port 57855 index 65669 
[3841495,NET]
SIP/2.0 200 OK
Via: SIP/2.0/TCP 10.16.169.162:57855;branch=z9hG4bK00003427
From: "1134" <sip:1134@198.18.133.3>;tag=bc542fe2ecfc06aa00005d75-00000afe
To: <sip:6@198.18.133.3;user=phone>;tag=1228730~0a37f347-c14b-44f5-8299-186aa2addafc-30514126
Date: Wed, 01 Feb 2023 15:18:21 GMT
Call-ID: bc542fe2-ecfc020b-00001560-0000274a@10.16.169.162
CSeq: 101 INVITE
Allow: INVITE, OPTIONS, INFO, BYE, CANCEL, ACK, PRACK, UPDATE, REFER, SUBSCRIBE, NOTIFY
Allow-Events: presence
Supported: replaces
Server: Cisco-CUCM12.5
Call-Info: <urn:x-cisco-remotecc:callinfo>; security= NotAuthenticated; orientation= to; gci= 1-17041; isVoip; call-instance= 1
Send-Info: conference, x-cisco-conference
Session-ID: 00007a4000105000a000005056a9a57c;remote=393bcdb429c149398852e75aa1228730
Remote-Party-ID: "Adam McKenzie - X6016" <sip:6016@198.18.133.3>;party=called;screen=yes;privacy=off
Contact: <sip:6@198.18.133.3:5060;transport=tcp>;+u.sip!devicename.ccm.cisco.com="UCSFAMCKENZIE";video;bfcp
Content-Type: application/sdp
Content-Length: 348

v=0
o=CiscoSystemsCCM-SIP 1228730 1 IN IP4 198.18.133.3
s=SIP Call
c=IN IP4 198.18.1.36
b=TIAS:8000
b=AS:24
t=0 0
a=cisco-mari:v1
a=cisco-mari-rate
m=audio 24632 RTP/AVP 18 101
a=extmap:14/sendrecv http://protocols.cisco.com/timestamp#100us
a=rtpmap:18 G729/8000
a=fmtp:18 annexb=no
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-15`~
```

