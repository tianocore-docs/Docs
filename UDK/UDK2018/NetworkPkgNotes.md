# [UDK2018]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2018) NetworkPkg  Notes

1. [New Features and Changes](#new-features-and-changes)
2. [Package Interface Changes](#package-interface-changes)
3. [UEFI/PI Specification Compliance](#uefi-pi-specification-compliance)
4. [Known Issues](#known-issues)


##                                               NEW FEATURES AND CHANGES
1. Update **network stack** drivers (`DHCP6, DNS, iSCSI, PXE `and `HTTP` Boot) to check
   the "`Network Media State`" type of AIP protocol, and wait the NIC to restore
   the network connection if `EFI_NOT_READY` is returned.

2. **HTTP Boot** Update
    1) Display more HTTP boot error messages which could help user to identify       the problem. For example, if an HTTP Redirect response is received, the       HTTP 3xx `Redirection` status code will be displayed, together with the new       URI address of the HTTP boot image.
    2) Add `EFI_HTTP_BOOT_CALLBACK_PROTOCOL` support.
    3) Add DNS device path node to the file path of an HTTP boot option.
    4) The "`IPv6 Support from UNDI`" type of AIP protocol is checked before start       HTTP Boot over IPv6.
    5) Fix a bug in HTTP Boot configuration form that an invalid URI address may       corrupt the setup screen.

3. **IP6 Update**
    1) Fix a bug that IP6 PXE boot option may disappear if IPv6 policy is changed.
    2) Update `Ip6Config.SetData()` to clean up certain configuration data.
    3) Fix a bug that `Ip6IpSecProcessPacket()` is always called to locate IpSec       protocol even the IpSec protocol has not been installed.
    
4. **HTTP Update**
    1) Add HTTP PATCH method support.
    2) Fix a bug that duplicate data in message body is returned to caller.

5. **iSCSI Update**
    1) Allow "`iSCSIISID:#`" keyword to be configured with a value of length 6 (only    last 3 bytes) or 12 (full ISID) according to the x-UEFI keyword definition in    http://uefi.org/confignamespace.
    2) Fix the incorrect maximum length of iSCSI keyword "`iSCSIInitiatorIpAddress:#`",       "`iSCSIInitiatorNetmask:#`", "`iSCSIInitiatorGateway:#`" and "`iSCSITargetIpAddress:#`".
    3) Remove the redundant '`/`' character in the returned "`ISCSIMacAddr`" keyword.
    4) Fix a bug that DHCP is started incorrectly if an iSCSI attempt is not       associated with the current NIC.
    5) The iSCSI configuration form is updated to display the initiator configuration       retrieved from DHCP service.
    6) The "`IPv6 Support from UNDI`" type of AIP protocol is checked before start       iSCSI boot over IPv6.
    7) Fix a bug in iSCSI configuration form that undeleted TargetIp address from       previous displayed attempt page will be saved to new created attempt. 
    8) Fix an iSCSI connection failure when Target info is first set to retrieve       from DHCP then static configured.
    9) Fix an iSCSI connection failure when the target info is retrieved from DHCP       and expressed as URI format. 
6. **DNS Update**
   1) Fix a bug that caller configured `RetryCount/RetryInterval` is not correct      used according to UEFI specification.

7. **DHCP6 Update**
    1) Media present status is checked before starting DHCP process.

8. **PXE Update**
    1) Fix a bug that incorrect return status code is used if no valid PXE offer       received in `PXE.Dhcp()` interface.
    2) Fix a bug in `PXE.SetStationIp()` that the `StationIp/SubnetMask` address should       not be modified if the input `NewStationIP/NewSubnetMask` parameter is `NULL`.
    3) Add warning debug message if PXE failed to read the system GUID from Smbios.


##  PACKAGE INTERFACE CHANGES
1. Remove `ping6` and `ifconfig6` shell application.<br>
Edk II has duplicated `ping6/ifconfig6` implementation in `NetworkPkg` and `ShellPkg`.    The usage and parameter format of these 2 versions are exactly the same. These    two commands have been added to UEFI Shell specification, so the copy under      `ShellPkg\Library\UefiShellNetwork2CommandsLib\`   will be actively maintained in future, and the `NetworkPkg` copy is deleted.


## UEFI PI SPECIFICATION COMPLIANCE
1. Add `EFI_HTTP_BOOT_CALLBACK_PROTOCOL` support as described in section 24.7.6 in
UEFI 2.7 A.    The HTTP Boot driver has been updated to support the EFI HTTP Boot `Callback`    Protocol. The new protocol will be invoked when the HTTP Boot driver is about    to transmit or has received a packet. A default implementation of the new     protocol to display the boot file download progress in percentage format will    be installed if the platform doesn't provide one.
   
2. UEFI 2.7 A adds clarification to `EFI_IP6_CONFIG_PROTOCOL.SetData()` to clear    certain data types with zero value of the DataSize parameter. The IP6 driver    is updated to support this feature.

3. The "`IPV6 Support from UNDI`" type is checked before starting IPv6 iSCSI/HTTP   Boot according to section 11.12.4 "`IPV6 Support from UNDI`" in UEFI 2.7 A.


## KNOWN ISSUES
1. iSCSI remote OS installation/boot with IPsec enabled.    In the current implementation, iSCSI driver is capable to access the storage    server in pre-OS environment when IPsec is enabled; while iSCSI remote OS    installation/boot cannot work with IPsec since the IPsec required parameters    are not conveyed to operating system. One possible way to handover such parameters    to the operating system is to use the GUIDed device path.

2. iSCSI NOP-Out package is not supported. 
In the current implementation, iSCSI NOP-Out feature is not supported. Upon    receipt of a NOP-In with the "`Target Transfer Tag`" set to a valid value rather    than default `0xFFFFFFFF`, the connection may break down. User need to close the    NOP-In feature in iSCSI target side to avoid the this failure.

3. iSCSI connection problem when IpSec IKEv2 enabled.
Due to the UEFI driver model limitation, the IpSec driver may have not been    started when iSCSI driver trying to initialize the connection with iSCSI target.    This will make iSCSI connection fail if IpSec IKEv2 is configured to use. User    can establish the iSCSI connection first before IpSec enabled to workaround this    issue.
   
4. Initiate PXE/HTTP Boot over Wi-Fi network immediately after system reset is not   supported.
The Wi-Fi device requires extra time to connect to AP, which couldn't be finished   before the boot options are loaded automatically. User could go to the Boot    Manager menu to select and start the PXE/HTTP Boot after Wi-Fi network is connected.

5. iSCSI connection problem over Wi-Fi network. 
Due to the UEFI driver model limitation, the Wi-Fi device can't connect to AP   when iSCSI driver trying to initialize the connection with iSCSI target. This    will make iSCSI connection fail over Wi-Fi network. User can load iSCSI driver    from UEFI shell after Wi-Fi network is connected to workaround this issue.


