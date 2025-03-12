[[CUCM]] Channel that allows to connect to other servers. Yellow lines ilustrates trunks.

![[sip trunk cluster.png]]

# SIP Offer/Answer Model

The offer message will include the following information: 

* Media Atributes (SDP)
* Codecs
* IP Address
* Port

Based on this information the destination will be sending his answer to the other side, then the media will flow.

There are basically two options for this model, Early offer and Delayed offer. The main difference is that the first one will be sending the set of capabilities in the first invite, the other one will be sending this invite without SDP.

**DEFAULT CONFIGURATION**
SIP Trunk ⇒ Delayed Offer && Supports Audio, Voice and Encrypted Calls

## Configuration

### MTP Required 

Allocate an [[MTP]] for every Single Call and share capabilites

The Next are configured on the SIP Profile of the Trunk, and they override the MTP Required.

![[MTP Required.png]]

Enabling this option on the SIP trunk assigns an MTP for every outbound call. This option does not support codec pass-through mode, which imposes a single codec (G.711 or G.729) limitation over the SIP trunk, thus limiting media to voice calls only. With this option enabled, calls over the trunk uses MTPs assigned to the trunk rather than using calling device MTPs, which forces the media to follow the same signaling path.

### Early Offer support for voice and video calls Mandatory (insert MTP if needed)

The trunk always will be sending Early Offer, however if the media capabilities of the sender cannot be determined it will allocate an MTP and will share the SDP of the [[MTP]]

Unified CM inserts an MTP to create an SDP Offer in the following cases only:

- An inbound call to Unified CM is received over a Delayed Offer SIP trunk
- An inbound call to Unified CM is received over an H.323 Slow Start trunk
- An inbound call is received from an older SCCP-based IP phone registered to Unified CM

![[Early Offer.png]]
For the above devices, Unified CM uses the media capabilities of the endpoint and applies the codec filtering rules based on the [[Regions]]-pair of the calling device and outgoing SIP trunk to create the offer SDP for the outbound SIP trunk call. In most cases, the offer SDP will have the IP address and port number of the endpoint initiating the call. This is assuming that Unified CM does not have to insert an MTP for other reasons such as a DTMF mismatch, TRP requirements, or a transcoder requirement when there is no common codec between the regions of the calling device and the SIP trunk.

Calls from: 

* Older SCCP-based phones
* SIP Delayed Offer trunks 
* H.323 Slow Start trunks 

The MTP will be allocated from the media resources associated with the calling device rather than from the outbound SIP trunk's media resources. (This prevents the media path from being hair-pinned via the outbound SIP trunk's MTP). If the MTP cannot be allocated from the calling device's media resource group list (MRGL), then the MTP allocation is attempted from the SIP trunk's MRGL.

For calls from:

* Older SCCP phones registered to Unified CM

Some of the media capabilities of the calling device (for example, supported voice codecs, video codecs, and encryption keys if supported) are available for media exchange through the Session Description Protocol (SDP). Unified CM will create a superset of the endpoint and MTP codec capabilities and apply the codec filtering based on the applicable region-pair settings. The outbound Offer SDP will use the MTP's IP address and port number and can support voice, video, and encrypted media. Note that a Cisco IOS-based MTP should be used and configured to support the pass-through codec.
### Early Offer support for voice and video calls Best Effort (no MTP inserted)

MTP will not be inserted, if delayed offer is received, delayed offer will be send, same for Early Offer

![[Best Effort.png]]

# DTMF Signaling

DMTF mismatch can be addresed by using MTP that will convert from one to one.

## Configuration

there are three options:

* DMTF Signaling Method: No Preference
This one will be trying to minimize the usage of MTP resources. 
[+] If both endpoints supports RFC2833 ⇒ No MTP will be invoked
[+] If both supports out-band then KPML will be use on the trunk ⇒ No MTP will be invoked
[+] If both supports either in-band and out-band then RFC 2833 will be the preference
[+] If there is a DTMF mismatch ⇒ MTP Will be invoked.

* DMTF Signaling Method: RFC 2833
[+] When at least one of the endpoints of the call does not supports RFC 2833 ⇒ MTP will be invoked
[+] If both endpoints supports RFC2833 ⇒ No MTP will be invoked

* DMTF Signaling Method: OOB and RFC 2833

Will NOT allocate MTP just when both devices support both methods.

# Secure

Securing SIP trunks involves two processes:

- Configuring the trunk to encrypt media
- Configuring the trunk to encrypt signaling

## Media Encryption

Media encryption can be configured on SIP trunks by checking the trunk's **SRTP allowed** check box. It is important to understand that enabling **SRTP allowed** causes the media for calls to be encrypted, but the trunk signaling will not be encrypted and therefore the session keys used to establish the secure media stream will be sent unencrypted. It is therefore important that you ensure that signaling between Unified CM and its destination SIP trunk device is also encrypted so that keys and other security-related information do not get exposed during call negotiations.

## Signaling Encryption

SIP trunks use TLS for signaling encryption. TLS is configured on the SIP Security Profile associated with the SIP trunk, and it uses X.509 certificate exchanges to authenticate trunk devices and to enable signaling encryption.

Certificates can be either of the following:

- Imported to each Unified CM node from every device that wishes to establish a TLS connection to that node's SIP trunk daemon
- Signed by a Certificate Authority (CA), in which case there is no need to import the certificates of the remote devices; only the CA certificate needs to be imported

Unified CM provides a bulk certificate import and export facility. However, for SIP trunks using **Run on all Active Unified CM Nodes** and up to 16 destination addresses, using a Certificate Authority provides a centralized and less administratively burdensome approach to setting up signaling encryption on SIP trunks.