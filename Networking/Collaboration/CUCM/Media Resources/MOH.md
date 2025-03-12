Music on hold (MoH) is an integral feature of the Cisco Unified Communications system. This feature provides music (or advertising) to callers when their call is placed on hold, transferred, parked, or added to an ad-hoc conference.

**Configure MOH

[+] Activate IPVMS

The Cisco IP Voice Media Streaming Application service must be Activated in order to use Music On Hold.

Once the IPVMS service is running on a Node, a MOH server will be created and added under

Media Resources > Music On Hold Server.

or 

``` powershell
admin:run sql select name, description from device where tkclass='12'

name  description
===== ==================
MOH_2 MOH_Trainingpub115
```

[+] For Custom MOH, we need to upload the file through MOH Audio File management, if we want to use default Audio from CUCM, we can skip this.

|   |   |
|---|---|
|**Step 1**|From Cisco Unified CM Administration, choose Media Resources > MOH Audio File Management.|
|**Step 2**|Click Upload File.|
|**Step 3**|Click Choose File and browse to the file you want to upload. Once you've selected the file, click Open.|
|**Step 4**|Click Upload.|
When you import an audio source file, Unified Communications Manager processes the file and converts the file to the proper formats for use by the Music On Hold server. Following are examples of valid input audio source files:

- 16-bit PCM .wav file
- Stereo or mono
- Sample rates of 48 kHz, 44.1 kHz, 32 kHz, 16 kHz, or 8 kHz

MOH audio source files do not automatically propagate to other MOH servers in a cluster. You must upload an audio source file to each MOH server or each server in a cluster separately

[+] Configure MOH Audio Source

Use this procedure to configure Music On Hold audio sources. You can configure audio streams and associate uploaded files to an audio stream. You can configure up to 500 audio streams.