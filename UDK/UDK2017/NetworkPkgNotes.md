# [UDK2017]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2017) NetworkPkg Notes



##  NEW FEATURES AND CHANGES
1. Update dual stack drivers (TCP, iSCSI, PXE, HTTP) to support IPv4 classless
   IP addressing.
2. Add TlsDxe driver to support EFI TLS Service Binding Protocol, EFI TLS
   Protocol and EFI TLS Configuration Protocol in UEFI 2.6.
3. HTTP Update
   1) Add HTTPS support (one-way authentication only.)
   2) Add HTTP PUT/POST method support.
   3) Support new error status code EFI_HTTP_ERROR in UEFI 2.6.
   4) Fix bugs in HTTP driver which will cause the second time HTTP Boot fail.
   5) Fix a bug that the HTTP completion token is not signaled if network
      transfer is failed.
   6) Fix a possible dead loop problem when receiving HTTP response message.
   7) Fix a bug that download big file (large than 4GB) through HTTP will fail.
4. HTTP Boot Update
   1) Add IPv6 support for UEFI HTTP Boot.
   2) Add RAM disk boot support for UEFI HTTP Boot.
   3) Add new configuration form for user to configure the HTTP Boot URI address
      for both home and corporate environment.
   4) Display the HTTP status code to screen when http layer occurs an error.
   5) Use the caller provided buffer directly in identity transfer-coding mode
      when download the boot image, in order to save the CopyMem operation to
      speed up the download performance.
   6) Fix a bug that unload HttpBootDxe driver may cause ASSERT.
   7) Fix the driver model issue in HttpBootDxe driver which may cause ASSERT.
   8) Fix a bug that HTTP Boot driver's HII_Config_Access.RouteConfig() function
      is not returned with correct status code.
   9) Fix a bug that the Boot image URI address is printed twice on screen.
   10) Clean up the HTTP/TCP resources before leaving HTTP Boot driver's EFI Load
      File Protocol, this will allow other programs to use these services.
   11) Check the Content-Type in response message headers for EFI HTTP boot image
      type (application/vnd.efi.img, application/vnd.efi.iso).
5. DNS Update
   1) Add GeneralLookUp() support for EFI DNS4/6 protocol.
   2) Check received packet size before use it to avoid ASSERT.
   3) Handle CNAME record in DNS response to support alias name.
   4) Check the RCODE field in DNS response and return EFI_NOT_FOUND accordingly.
   5) Accept zero StationIp in DNS configuration, so IP6 driver will take
      responsibility to choose a proper station address.
6. PXE Update
   1) Support domain name represented boot file URI option (DHCP option 59).
   2) Use notify event instead of timeout event when setting IPv6 address to
      reduce the waiting time.
   3) Reset DHCP4 child before leaving PXE driver's EFI Load File Protocol, this
      will allow other programs (like HTTP boot) to use DHCP4 service in future.
   4) Fix a bug that PXE10/WFM11a offer is dropped incorrectly.
   5) Fix a PXE boot failure issue when a DhcpBinl offer is received.
7. IP6 Update
   1) Change the default IPv6 configuration policy to Ip6ConfigPolicyManual.
   2) Fix a bug in Ip6CleanService() which may lead to CPU exception.
   3) Clean up the previous DNS configuration if IP policy is changed.
   4) Update the Ethernet interface name from hexadecimal number to decimal (e.g.
      ethA -> eth10, ethB -> eth11) for EFI IPv6 Configuration Protocol.
   5) Remove the duplicated DNS address check for EFI IPv6 Configuration Protocol.
   6) Ignore unnecessary DAD callback after IP policy is changed, which may cause
      ASSERT in PXE IPv6 boot.
8. UDP6 Update
   1) Check received packet size before use it to avoid ASSERT.
9. TCP Update
   1) Add Cancel() interface support for EFI TCP Protocol.
   2) Addressing TCP Window Retraction problem when window scale factor is used.
   3) Check received packet size before use it to avoid ASSERT.
   4) Fix bug in TCP which not sending out ACK in certain circumstance.
   5) Update TCP to use EFI_D_NET for DEBUG message.
10. iSCSI Update
    1) Check the EFI Adapter Information Protocol (AIP) before starting iSCSI.
    2) Support bracketed IPv6 address during a iSCSI redirection.
    3) Support domain name represented iSCSI target URL configuration.
    4) Support UEFI keyword configuration through EFI Configuration Keyword
      Handler Protocol.
    5) Update the driver to record the user configured TargetIP/Port in iBFT
      table, instead of the redirected IP address if iSCSI redirection occurs.
    6) Fix a bug in iSCSI MPIO that ExtScsiPassThruProtocol is uninstalled twice.
    7) Fix a bug that iSCSI couldn't create new session with Window iSCSI
      target after "reconnect -r".
11. DHCP6 Update
    1) Use DUID-LLT if UUID is not provided by SMBIOS table.
    2) Check received packet size before use it to avoid ASSERT.
12. IpSec Update
    1) Update SPD and SAD entry mapping when SPD is updated.
    2) Fix UEFI IKE Initial Exchange failure.
    3) Generate SPI randomly and correct IKE_SPI_BASE value.
13. IpSecConfig Application Update
    1) Handle the SPC and SAD entry mapping change when SPD is updated.


##  PACKAGE INTERFACE CHANGES
1. New added network stack modules.<BR>
     NetworkPkg/TlsDxe/TlsDxe.inf<BR>
     NetworkPkg/TlsAuthConfigDxe/TlsAuthConfigDxe.inf<BR>
2. New PCD PcdIScsiAIPNetworkBootPolicy is introduced for platform owner to
   define particular policy when the iSCSI HBA will survive and UEFI iSCSI will
   fail. According to UEFI spec, iSCSI HBA must install an AIP instance with
   network boot information block. The iSCSI driver has been updated to check
   whether there are AIP instances installed by iSCSI HBA adapter.
   The default policy is STOP_UEFI_ISCSI_IF_AIP_SUPPORT_OFFLOAD (0x08), which
   means that when ISCSI HBA adapter installs an AIP and claims it supports an
   offload engine for iSCSI boot, the UEFI iSCSI driver will not start to manage
   that device.
   The interoperability test for validating UEFI iSCSI co-work with particular
   iSCSI HBA is not fully covered since lack of iSCSI HBA devices which provide
   such feature.
3. New PCD PcdAllowHttpConnections to define the security policy that whether HTTP
   connections are forbidden or not. This PCD won't impact the HTTPS connection
   which is always allowed.

##  UEFI / PI SPECIFICATION COMPLIANCE
1. Add new protocol support for TLS.<BR>
     EFI_TLS_SERVICE_BINDING_PROTOCOL<BR>
     EFI_TLS_PROTOCOL<BR>
     EFI_TLS_CONFIGURATION_PROTOCOL<BR>
2. Add HTTP Boot support for home environment as described in section 23.7.2.2
   "Use case in Home environment" in UEFI 2.6. HTTP Boot driver is updated to
   produce a "HTTP Boot Configuration" setup page for user to create HTTP boot
   options with manually entered URI address.
3. Add RAM disk support for HTTP Boot as described in section 23.7 "HTTP Boot" in
   UEFI 2.6. The HTTP Boot server could provide a RAM disk image other than an
   UEFI executable image for the client to use as a "Network Boot Program"(NBP).
   The RAM disk image must contain a UEFI compliant file system (section 12.3 of
   UEFI 2.6) in it. HTTP boot driver will identify the image type either by the
   mime type in server response "Content-Type" header or by the file name
   extension as below:<BR>
     "application/efi"         or *.efi  ->  EFI Image<BR>
     "application/vnd.efi-iso" or *.iso  ->  CD/DVD Image<BR>
     "application/vnd.efi-img" or *.img  ->  Virtual Disk Image<BR>
   Please note that the BDS is required to be updated for properly handling the
   EFI_WARN_FILE_SYSTEM status code from HTTP Boot driver, otherwise the HTTP
   RAM disk boot will not work.
4. TCP driver is updated to support below Cancel() interface.<BR>
     EFI_TCP4_PROTOCOL.Cancel()<BR>
     EFI_TCP6_PROTOCOL.Cancel()<BR>
5. Add x-UEFI-ns configuration language namespace support for iSCSI as described
   in section 33.3 "EFI Configuration Keyword Handler Protocol" in UEFI 2.6.


##  KNOWN ISSUES
1. iSCSI remote OS installation/boot with IPsec enabled.
   In the current implementation, iSCSI driver is capable to access the storage
   server in pre-OS environment when IPsec is enabled; while iSCSI remote OS
   installation/boot cannot work with IPsec since the IPsec required parameters
   are not conveyed to operating system. One possible way to handover such parameters
   to the operating system is to use the GUIDed device path.

2. System ASSERT when press "ESC" in WDS "PXE windows Boot manager" screen.
   The issue only happens on a DEBUG build firmware. When use some version of the
   Windows WDS loader (wdsmgfw.efi), like from Windows 10, the content of its
   EFI_LOADED_IMAGE_PROTOCOL will be disrupted and cause this ASSERT. User could
   avoid pressing "ESC" during the windows boot loader phase to avoid the crash,
   or using the wdsmgfw.efi from a workable Windows version.

3. iSCSI NOP-Out package is not supported.
   In the current implementation, iSCSI NOP-Out feature is not supported. Upon
   receipt of a NOP-In with the "Target Transfer Tag" set to a valid value rather
   than default 0xFFFFFFFF, the connection may break down. User need to close the
   NOP-In feature in iSCSI target side to avoid the this failure.

4. iSCSI connection problem when IpSec IKEv2 enabled.
   Due to the UEFI driver model limitation, the IpSec driver may have not been
   started when iSCSI driver trying to initialize the connection with iSCSI target.
   This will make iSCSI connection fail if IpSec IKEv2 is configured to use. User
   can establish the iSCSI connection first before IpSec enabled to workaround this
   issue.

