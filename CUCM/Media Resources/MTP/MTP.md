A media termination point (MTP) is an entity that accepts two full-duplex media streams. It bridges the streams together and allows them to be set up and torn down independently. The streaming data received from the input stream on one connection is passed to the output stream on the other connection, and vice versa. MTPs have many possible uses, such as:

- Re-Packetization of a Stream
- DTMF Conversion
- Protocol-specific usage (bridging between IPv4 and IPv6 endpoints)

–![](https://www.cisco.com/c/dam/en/us/td/i/templates/blank.gif) Calls over SIP Trunks

–![](https://www.cisco.com/c/dam/en/us/td/i/templates/blank.gif) H.323 Supplementary Services
–![](https://www.cisco.com/c/dam/en/us/td/i/templates/blank.gif) H.323 Outbound Fast Connect

Reasons To Allocate an MTP:
[[Estructura]]
- To deliver a SIP Early Offer over SIP trunks
- To address [[DTMF]] transport mismatches
- To act as an RSVP agent
- To act as a Trusted Relay Point (TRP)
- To provide conversion between IPv4 and IPv6 for RTP streams

Available From:

* Hardware, MTP on IOS using DSP resources.
* Software, MTP on IOS on ASR 1000 with RP2 Processor
* Software, on CUCM where IPVMS is running.

