```sql
insert into device (tkDeviceProtocol, fkMatrix_Presence, tkPacketCaptureMode, RetryVideoCallAsAudio, usedevicepoolcgpntransformcss, tkStatus_MLPPIndicationStatus, tkstatus_audiblealertingbusy, tkProtocolSide, DeviceLevelTraceFlag, remotedevice, 11 fkPhoneTemplate, RFC2833Disabled, srtpallowed, tkoutboundcallrollover, subunit, 16 fkcommonphoneconfig, tkdevicetrustmode, tkStatus_BuiltInBridge, 19description, sshuserid, tkstatus_alwaysuseprimeline, requirecerlocation, AllowHotelingFlag, unattended_Port, isProtected, ignorepi, tkCertificateOperation, tkDNDOption, fkLocation, isActive, sshpassword, tkphonepersonalization, enableixchannel, 34 pkid, tkDeviceProfile, tkStatus_UseTrustedRelayPoint, tkCountry, tkProduct, MTPRequired, tkCertificateStatus, 41 tkModel, tkphoneservicedisplay, usedevicepoolcgpningressdn, requireDTMFReception, 45 fkDevicePool, packetCaptureDuration, tkringsetting_dnd, enablebfcp, calreference, 50 name, fkSIPProfile, enablecallroutingtordwhennoneisactive, tkstatus_devicemobilitymode, tkPreemption, hotlinedevice, tkstatus_joinacrosslines, tkbarge, tkstatus_audiblealertingidle, allowCTIControlFlag, unit, tkUserLocale, tkClass, tkstatus_alwaysuseprimelineforvm, specialLoadInformation, earlyoffersupportforvoicecall, fkSecurityProfile) values (11, (select pkid from matrix where name = "Standard Presence group ")'ad243d17-98b4-4118-8feb-5ff2e1b781ac', 0, 't', 't', 2, 2, 1, 'f', 'f', 11 (select pkid from phonetemplate where name = "Standard 8865 SIP")'6f12c330-12b3-d725-06db-983b38a06a12', 'f', 'f', 0, 0, 16 (select pkid from commonphoneconfig where name = "Standard Common Phone Profile Standard Common Phone Profile") 'ac243d17-98b4-4118-8feb-5ff2e1b781ac', 0, 2, 19 'SEP189C5DB76600', '', 2, 'f', 'f', 'f', 'f', 'f', 1, 2, '29c5c1c4-8871-4d1e-8394-0b9181e8c54d', 't', '', 3, 'f', 34 newPkid '8aedce0e-c913-cc53-3198-d70b9c069fdc', 0, 2, NULL, 38 (select enum from typemodel where name = "Cisco 8865")36678, 'f', 1, 41 (select enum from typemodel where name = "Cisco 8865")36225, 3, 't', 'f', 45 (select pkid from devicepool where name = "default")'1b1b9eb6-7803-11d3-bdf0-00108302ead1', 0, NULL, 'f', -1, 50 'SEP189C5DB76600', 'fcbc7581-4d8d-48f3-917e-00b09fb39213', 'f', 2, 2, 'f', 0, 0, 2, 't', 0, NULL, 1, 2, '', 'f', (select pkid from securityprofile where name = "Cisco 8865 - Standard SIP Non-Secure Profile")'caaa9b20-ecc5-4fa8-ab1c-77325d8e418c')
```

```sql
insert into device (tkDeviceProtocol, fkMatrix_Presence, tkPacketCaptureMode, RetryVideoCallAsAudio, usedevicepoolcgpntransformcss, tkStatus_MLPPIndicationStatus, tkstatus_audiblealertingbusy, tkProtocolSide, DeviceLevelTraceFlag, remotedevice, fkPhoneTemplate, RFC2833Disabled, srtpallowed, tkoutboundcallrollover, subunit, fkcommonphoneconfig, tkdevicetrustmode, tkStatus_BuiltInBridge, description, sshuserid, tkstatus_alwaysuseprimeline, requirecerlocation, AllowHotelingFlag, unattended_Port, isProtected, ignorepi, tkCertificateOperation, tkDNDOption, fkLocation, isActive, sshpassword, tkphonepersonalization, enableixchannel, tkDeviceProfile, tkStatus_UseTrustedRelayPoint, tkCountry, tkProduct, MTPRequired, tkCertificateStatus, tkModel, tkphoneservicedisplay, usedevicepoolcgpningressdn, requireDTMFReception, fkDevicePool, packetCaptureDuration, tkringsetting_dnd, enablebfcp, calreference, name, fkSIPProfile, enablecallroutingtordwhennoneisactive, tkstatus_devicemobilitymode, tkPreemption, hotlinedevice, tkstatus_joinacrosslines, tkbarge, tkstatus_audiblealertingidle, allowCTIControlFlag, unit, tkUserLocale, tkClass, tkstatus_alwaysuseprimelineforvm, specialLoadInformation, earlyoffersupportforvoicecall, fkSecurityProfile, pkid) values (11, (select pkid from matrix where name = "Standard Presence group"), 0, 't', 't', 2, 2, 1, 'f', 'f', (select pkid from phonetemplate where name = "Standard 8865 SIP"), 'f', 'f', 0, 0, (select pkid from commonphoneconfig where name = "Standard Common Phone Profile"), 0, 2, 'SEP189C5DB76600', '', 2, 'f', 'f', 'f', 'f', 'f', 1, 2, '29c5c1c4-8871-4d1e-8394-0b9181e8c54d', 't', '', 3, 'f', 0, 2, NULL, (select enum from typemodel where name = "Cisco 8865"), 'f', 1, (select enum from typemodel where name = "Cisco 8865"), 3, 't', 'f', (select pkid from devicepool where name = "Default"), 0, NULL, 'f', -1, 'SEP189C5DB76600', 'fcbc7581-4d8d-48f3-917e-00b09fb39213', 'f', 2, 2, 'f', 0, 0, 2, 't', 0, NULL, 1, 2, '', 'f', (select pkid from securityprofile where name = "Cisco 8865 - Standard SIP Non-Secure Profile"), Pkid())
```