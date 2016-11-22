========================================================================================================================
                                                     FEATURES LIST
========================================================================================================================
NetworkPkg provides IPv6 network stack drivers, IPsec driver, PXE driver, iSCSI driver and necessary shell applications
for network configuration.

1.  IPv6 network stack
    1)  NetworkPkg/Ip6Dxe - Ip6 driver, which produces
        EFI_IP6_PROTOCOL
        EFI_IP6_CONFIG_PROTOCOL
    2)  NetworkPkg/Udp6Dxe Udp6 driver, which produces
        EFI_UDP6_PROTOCOL
    3)  NetworkPkg/TcpDxe - TCP combo driver, which produces
        EFI_TCP4_PROTOCOL
        EFI_TCP6_PROTOCOL
    4)  NetworkPkg/Dhcp6Dxe - DHCP6 driver, which produces
        EFI_DHCP6_PROTOCOL
    5)  NetworkPkg/Mtftp6Dxe - MTFTP6 driver, which produces
        EFI_MTFTP6_PROTOCOL

2.  IPsec
    NetworkPkg/IpSecDxe - Ipsec driver, which produces
      EFI_IPSEC2_PROTOCOL
      EFI_IPSEC_CONFIG_PROTOCOL

3.  PXE
    NetworkPkg/UefiPxeBcDxe - PXE combo driver, which produces
      EFI_PXE_BASE_CODE_PROTOCOL
      EFI_LOAD_FILE_PROTOCOL

4.  iSCSI
    NetworkPkg/IScsiDxe - iSCSi driver, which produces:
      EFI_ISCSI_INITIATOR_NAME_PROTOCOL
      EFI_EXT_SCSI_PASS_THRU_PROTOCOL
      EFI_AUTHENTICATION_INFO_PROTOCOL

5.  Shell Applications
    1)  NetworkPkg/Application/Ping6
        This command is used to ping the target host with IPv6 stack.
    2)  NetworkPkg/Application/IfConfig6
        This command is used to configure the default IP address of the UEFI IPv6.
    3)  NetworkPkg/Application/IpSecConfig
        This command is used to create, edit and delete the IPsec policy, i.e.
        Security Policy Database (SPD), Security Association Database (SAD),
        and Peer Authentication Database (PAD).
    4)  NetworkPkg/Application/VConfig
        This command is used to configure VLAN of a network interface.

Note: UNDI, SNP, DPC and MNP drivers are components shared by IPv4 network stack and IPv6 network stack. These modules
      could be found in MdeModulePkg  except the UNDI driver. For UNDI driver, please contact the Card Vendor.

Note: The PXE driver (NetworkPkg/UefiPxeBcDxe) supports boot on both IPv4 and IPv6 network stack.

Note: Supported features in IPsec
      1)  Security Protocols          Encapsulating Security Payload (ESP)
      2)  IPsec Mode                  Transport/Tunnel mode
      3)  Encryption Algorithm        3DES-CBC, AES-CBC
      4)  Authentication Algorithm    HMAC_SHA1_96
      5)  Authentication Method       Pre-shared Key, X509 Certificates

Note: After IPsec is enabled in both side, all inbound and outbound IP packet are processed by IPsec.

========================================================================================================================
                                                      FEATURES ENABLING
========================================================================================================================
1.  Libraries used by network modules (for both v4 and v6):

      DpcLib|MdeModulePkg/Library/DxeDpcLib/DxeDpcLib.inf
      NetLib|MdeModulePkg/Library/DxeNetLib/DxeNetLib.inf
      IpIoLib|MdeModulePkg/Library/DxeIpIoLib/DxeIpIoLib.inf
      UdpIoLib|MdeModulePkg/Library/DxeUdpIoLib/DxeUdpIoLib.inf
      TcpIoLib|MdeModulePkg/Library/DxeTcpIoLib/DxeTcpIoLib.inf

2.  Modules shared between IPv4 network stack and IPv6 network stack:

      MdeModulePkg/Universal/Network/SnpDxe/SnpDxe.inf
      MdeModulePkg/Universal/Network/Dpc/Dxe/Dpc.inf
      MdeModulePkg/Universal/Network/MnpDxe/MnpDxe.inf
      MdeModulePkg/Universal/Network/VlanConfigDxe/VlanConfigDxe.inf

Note: platform/NIC specific UNDI driver is also required for network stack enabling.

3.  IPv4 network stack modules:

      MdeModulePkg/Universal/Network/ArpDxe/ArpDxe.inf
      MdeModulePkg/Universal/Network/Ip4ConfigDxe/Ip4ConfigDxe.inf
      MdeModulePkg/Universal/Network/Ip4Dxe/Ip4Dxe.inf
      MdeModulePkg/Universal/Network/Udp4Dxe/Udp4Dxe.inf
      MdeModulePkg/Universal/Network/Dhcp4Dxe/Dhcp4Dxe.inf
      MdeModulePkg/Universal/Network/Mtftp4Dxe/Mtftp4Dxe.inf
      MdeModulePkg/Universal/Network/Tcp4Dxe/Tcp4Dxe.inf

4.  IPv6 network stack modules:

      NetworkPkg/Ip6Dxe/Ip6Dxe.inf
      NetworkPkg/Udp6Dxe/Udp6Dxe.inf
      NetworkPkg/Dhcp6Dxe/Dhcp6Dxe.inf
      NetworkPkg/Mtftp6Dxe/Mtftp6Dxe.inf
      NetworkPkg/TcpDxe/TcpDxe.inf

5.  Dual network stack (both v4 and v6) modules:

      MdeModulePkg/Universal/Network/ArpDxe/ArpDxe.inf
      MdeModulePkg/Universal/Network/Ip4ConfigDxe/Ip4ConfigDxe.inf
      MdeModulePkg/Universal/Network/Ip4Dxe/Ip4Dxe.inf
      MdeModulePkg/Universal/Network/Udp4Dxe/Udp4Dxe.inf
      MdeModulePkg/Universal/Network/Dhcp4Dxe/Dhcp4Dxe.inf
      MdeModulePkg/Universal/Network/Mtftp4Dxe/Mtftp4Dxe.inf
      NetworkPkg/Ip6Dxe/Ip6Dxe.inf
      NetworkPkg/Udp6Dxe/Udp6Dxe.inf
      NetworkPkg/Dhcp6Dxe/Dhcp6Dxe.inf
      NetworkPkg/Mtftp6Dxe/Mtftp6Dxe.inf
      NetworkPkg/TcpDxe/TcpDxe.inf

Note:
    1)  Tcp4Dxe (MdeModulePkg/Universal/Network/Tcp4Dxe/Tcp4Dxe.inf) only support IPv4.
    2)  TcpDxe (NetworkPkg/TcpDxe/TcpDxe.inf) support both IPv4 and IPv6.
    3)  Either Tcp4Dxe or TcpDxe could be used, but they should not be used at the
        same time.

6.  PXE drivers
    There are two PXE drivers, one is in MdeModulePkg (support IPv4 only):

      MdeModulePkg/Universal/Network/UefiPxeBcDxe/UefiPxeBcDxe.inf

    and the other is in NetworkPkg (support both IPv4 and IPv6):

      NetworkPkg/UefiPxeBcDxe/UefiPxeBcDxe.inf

Note:
    1)  MdeModulePkg/Universal/Network/UefiPxeBcDxe/UefiPxeBcDxe.inf support only IPv4.
    2)  NetworkPkg/UefiPxeBcDxe/UefiPxeBcDxe.inf support both IPv4 and IPv6
    3)  Either PXE driver in MdeModulePkg or NetworkPkg could be used, but they
        should not be used at the same time.

7.  iSCSI
    iSCSI driver locates between the IP stack and the SCSI stack. In this way, a file or a hard disk of an iSCSI server
    machine can be mapped as a SCSI hard disk of client machine. To enable iSCSI functionality the following drivers
    and are required.

    1)  IPv4 stack or IPv6 stack
        Please refer to item3 and item4 for details.
    2)  SCSI stack
        MdeModulePkg/Bus/Scsi/ScsiBusDxe/ScsiBusDxe.inf
        MdeModulePkg/Bus/Scsi/ScsiDiskDxe/ScsiDiskDxe.inf

Note: iSCSI driver works on dual network stacks (IPv4 stack and IPv6 stack), whether to use the IPv4 stack or the IPv6
      stack depends on its UI configuration from the administrator.

Note: Only one instance of iSCSI driver is allowed in a platform. Either to use iSCSI driver in
      MdeModulePkg/Universal/Network/IScsiDxe/IScsiDxe.inf or this driver in NetworkPkg/IScsiDxe/IScsiDxe.inf is up to
      platform administrator.

Note: The iSCSI typical usage model (UEFI system as an iSCSI initiator) includes:
      1)  Working in single path mode and multipath I/O mode.
      2)  Working by requesting DHCP for IP address and target info or static configuration.
      3)  Login with CHAP or None authentication method.

8.  IPsec driver
    To enable IPsec driver the following steps should be followed:
    1)  Make sure the IPsec module INF is added to platform DSC/FDF file.
        NetworkPkg/IpSecDxe/IpSecDxe.inf

    2)  IPsec driver could co-work with IPv4 network stack or IPv6 network stack or dual stack (both v4 and v6). The
        default status of IPsec is disabled and IPsec feature could be enabled by ipsecConfig tool.

9.  VLAN
    Software VLAN is implemented in module MnpDxe in MdeModulePkg, i.e. VLAN tag is inserted or removed by software.
    Since some NIC hardware support VLAN feature, i.e. the VLAN tag will be inserted or removed by hardware, the UNDI
    driver may produce EFI_VLAN_CONFIG_PROTOCOL for VLAN support. The MnpDxe will check whether EFI_VLAN_CONFIG_PROTOCOL
    is already produced by UNDI driver, if yes, then MnpDxe will not provide software VLAN implementation; if no,
    then MnpDxe will provide its software VLAN implementation.

    The module VlanConfigDxe in MdeModulePkg produce HII Config Form used for VLAN configuration in UI, it will consume
    the EFI_VLAN_CONFIG_PROTOCOL. If VLAN UI configure Form is not required, then module VlanConfigDxe could be removed
    from platform DSC/FDF file, and the Shell application vconfig could be used for VLAN configuration equivalently.

10. Shell Applications
    1)  NetworkPkg provides 4 shell applications:

        NetworkPkg/Application/Ping6/Ping6.inf
        NetworkPkg/Application/IfConfig6/IfConfig6.inf
        NetworkPkg/Application/IpsecConfig/IpSecConfig.inf
        NetworkPkg/Application/VConfig/VConfig.inf

    2)  These shell applications depend (but not limited) on following Libraries for build:

        DebugLib|MdePkg/Library/UefiDebugLibStdErr/UefiDebugLibStdErr.inf
        FileHandleLib|ShellPkg/Library/UefiFileHandleLib/UefiFileHandleLib.inf
        SortLib|ShellPkg/Library/UefiSortLib/UefiSortLib.inf
        ShellLib|ShellPkg/Library/UefiShellLib/UefiShellLib.inf

    3)  ping6 and ifconfig6 depend on EFI_IP6_PROTOCOL and EFI_IP6_CONFIG_PROTOCOL
        provided by IP6 driver, so make sure Ip6Dxe is started before use.

    4)  ipsecconfig application depends on EFI_IPSEC2_PROTOCOL and EFI_IPSEC_CONFIG_PROTOCOL
        provided by IPsec driver, so make sure IpSecDxe is started before use.

    5)  vconfig application depends on EFI_VLAN_CONFIG_PROTOCOL provided by MNP
        driver, so make sure MNP driver is started before use.

11. Validation using NT32 platform
    To validate network stack on NT32 platform, please refer to document "UEFI Network Stack for EDK Getting Started Guide"
    (EFINetworkStackGettingStarted.pdf from http://sourceforge.net/projects/network-io/files/Documents/).

========================================================================================================================
                                                   SHELL APPLICATIONS USAGE
========================================================================================================================
NetworkPkg provides some shell applications to configure IPv6 network stack and IPsec Drivers. There are the usage and
examples of these applications.

1.  Ping6  Ping the target host with IPv6 stack.
    Usage: ping6 [-n count] [-l size] [-s sourceaddress] TargetIp

           -n         Number of echo request datagrams to be sent.
           -l         Size of data buffer in the echo request datagram.
           -s         Source address of the selected interface. When TargetIp is
                      a link-local address, this option is required.
           TargetIp   IPv6 address of the target machine.

    Example: To ping the target host by sending 20 echo request datagram from the
             interface with fe80::3 link-local address.
             Shell:\> ping6 -n 20 -s fe80::3 fe80::1


2.  ifconfig6 Modify the default IP address of the UEFI IPv6 Network Stack
    usage: ifConfig6 [-?] [-r [Name]] [-l [Name]] [-s <Name> command...]

           -r [Name]  Renew the configuration for all or specified interface and
                      set automatic policy.
           -l [Name]  List the configuration for all or the specified interface.

           -s < Name> command...   Set configuration of interface as follows.
                  |man/auto    manual or automatic policy
                  |id  {mac}   alternative interface id.
                  |dad {num}   dad transmits count.
                  |host{ip}    static host ip address, must under manual policy.
                  |gw  {ip}    gateway ip address, must under manual policy.
                  |dns {ip}    dns server ip address, must under manual policy.

           -?         Display the help message

           Note: [Name] points to Adapter name, i.e., eth0

    Example1: To use the static IP6 addresses configuration for the interface eth0,
              and this configuration survives the network reload:
              Shell:\> ifconfig6 -s eth0 man host 2002::1/64 2002::2/64 gw 2002::3/64

    Example2: To set the alternative interface id of eth0 under manual policy:
              Shell:\> ifconfig6 -s eth0 man id ff:dd:aa:88:66:cc

    Example3: To set the dad transmits count for eth0 under automatic policy
              Shell:\> ifconfig6 -s eth0 auto dad 10


3.  vconfig  Add or remove a VLAN for a specified network interface
    usage: vConfig [-?] [-a Name VlanId [Priority]] [-d Name.VlanId] [-l [Name]]

       -a Name VlanId [Priority]     Add a VLAN for a specified interface.
       -d Name.VlanId                Remove a VLAN for a specified interface.
       -l [Name]                     List the VLAN configuration for all, or a specified interface.
       -?                            Display the help message.

    Example1: To create a VLAN for interface eth0 with VLAN ID 1000 and Priority 7:
              Shell:\> vconfig -a eth0 1000 7
              Note: If the specified VLAN ID has already been configured for this
                    interface, this command updates the priority for the VLAN.

    Example2: To delete the VLAN 1024 of interface eth0:
              Shell:\> vconfig -d eth0.1024

    Example3: To create a VLAN with VLAN ID 1024 for interface eth0:
              Shell:\> vconfig -a eth0 1024
              Note: If priority is not specified in command line, the newly
              created VLAN will be set with default priority 0.

4.  ipsecconfig ipsecconfig.efi is a shell application that can be used to create, edit and delete the IPsec policy(SPD/SAD/PAD).

    usage: ipsecconfig [-p <SAD|SPD|PAD>] [command] [options[parameters]]
           Note: The arguments of ipsecconfig tool are case sensitive

           [command] is one of the command flags that specify an operation to perform on an executable.
                     The following commands are supported by ipsecconfig:

                     Command                             Description
                     - l                                 Require '-p' option. List all entries for specified database
                     - d  entryid                        Require '-p' option. Delete specified entry matching the
                                                         entryid in specified database
                     - e entryid [options [parameters]]  Require '-p' option. Edit specified entry matching the
                                                         entryid in specified database
                     - a options [parameter]]            Require '-p' option. Add new policy entry in specified database
                     - i entryid [options[parameters]]   Require '-p' option. Insert new policy entry before the one
                                                         matching the entryid in specified database.
                     - f                                 Require '-p' option. Flush specified database.
                     - Enable                            Enable IPsec.
                     - Disable                           Disable IPsec.
                     - Status                            Show the current IPsec status - enabled or disabled.
                     - ?                                 For help and usage information

           [options [parameters]]   describes one or more command options and/or its desired parameters.

           1)  Options for SPD are designed as follows:
               Options                          Description
               --local localaddress             Required. Local Ip address. Support both Ipv4
                                                and Ipv6 address. By default prefix, length is 128 for IPv6 and
                                                32 for IPv4. 0.0.0.0/0 is any address for IPv4. ::/0 is any address
                                                for Ipv6.
               --remote remoteaddress           Required. Remote Ip address. Support both Ipv4 and Ipv6 address.
                                                By default prefix, length is 128 for IPv6 and 32 for IPv4. 0.0.0.0/0 is
                                                any address for IPv4. ::/0 is any address for Ipv6.
               --proto protocol                 Required. Next layer protocol. Support both string and integer.
                                                Support Strings include UDP, TCP, and ICMP. 0xffff for any protocol.
                                                protocol=<UDP|TCP|ICMP|...>. Refer to RFC 1700 for protocol number
                                                assignment.
               --local-port port                Optional. Next layer port for local. 0 is by default for any port.
               --remote-port port               Optional. Next layer port for remote. 0 is by default for any port.
               --action action                  Required. action=<bypass|discard|protect>.
               --name name                      Optional. SPD name.
               --mode mode                      Optional. mode=<transport|tunnel>. Transport by default.
               --tunnel-local tunnellocaladdr   Optional. tunnel local address(only for tunnel mode).
               --tunnel-remote tunnelremoteaddr Optional. tunnel remote address(only for tunnel mode).
               --ipsec-proto iProtocol          Optional. iProtocol=<AH|ESP>. ESP by default.
               --auth-algo alg                  Optional. Authentication algorithm. algo=<SHA1HMAC|NONE>
               --encrypt-algo algo              Optional. Encrypt algorithm. Algo=<AESCBC|3DESCBC|NONE>

           2)  Options for SAD are designed as follows:
               Options                          Description
               --spi spi                        Required. Security Parameter Index.
               --ipsec-proto iProtocol          Required. iProtocol=<AH|ESP>.
               --local localaddress             Required. Local Ip address. Support both Ipv4
                                                and Ipv6 address. By default prefix, length is 128 for IPv6 and
                                                32 for IPv4. 0.0.0.0/0 is any address for IPv4. ::/0 is any address
                                                for Ipv6.
               --remote remoteaddress           Required. Remote Ip address. Support both Ipv4 and Ipv6 address.
                                                By default prefix, length is 128 for IPv6 and 32 for IPv4. 0.0.0.0/0 is
                                                any address for IPv4. ::/0 is any address for Ipv6.

               --encrypt-algo algo              Required for ESP. Encrypt algorithm. Algo=<AESCBC|3DESCBC|NONE>
               --encrypt-key key                Required for ESP. Encrypt key. String type.
               --auth-algo algo                 Required for ESP and AH. Authentication algorithm. algo=<SHA1HMAC|NONE>
               --auth-key key                   Required for ESP and AH. Authentication key. String type.
               --mode mode                      Optional. mode=<transport|tunnel>. Transport by default.
               --tunnel-local tunnellocaladdr   Optional. tunnel local address(only for tunnel mode).
               --tunnel-remote tunnelremoteaddr Optional. tunnel remote address(only for tunnel mode).

               Note: To set a manual SAD, the related SPD options of local address, remote address, IP protocol ID
                     and Action should also be set, because these fields is used for SPD selector creation.
                     IPsec Configration Protocol will associate this SA with a SPD according to these fields. It
                     also need to make sure there is SPD which has the same SPD selector with the one set in SAD.

           3)  Options for PAD are designed as following:
               Note: The PAD configuration does not influence IPsec behavior since IKE is not supported in this release.

               Options                       Description
               --peer-address address        Required. Peer address. Support both Ipv4 and Ipv6 address. By default
                                             prefix, length is 128 for IPv6 and 32 for IPv4. 0.0.0.0/0 is any
                                             address for IPv4. ::/0 is any address for Ipv6.
               --auth-proto ikeproto         Optional. IKE protocol version. Ikeproto=<IKEv1|IKEv2>. IKEv1 by default.
               --auth-method method          Required. Authentication method. method=<PreSharedSecret|Certificates>.
               --auth-data data              Required. Data for authentication. String type.

    Example1: Set a SPD to protect the ICMP6 packet between fec0::200:1 and fec0::200:2
              Shell:\> ipsecconfig -p SPD -a --local fec0::200:2 --remote fec0::200:1 --proto 58 --action Protect

    Example2: Set a SPD to protect all packets between 2000:aaaa::1 and 2000:aaaa::2
              Shell:\> ipsecconfig -p SPD -a --local 2000:aaaa::1 --remote 2000:aaaa::2 --proto 0xffff
                       --action Protect --ipsec-proto ESP --mode Transport

    Example3: Set a SAD entry related to the SPD which used to protect the packet between 2000:aaaa::1 and 2000:aaaa::2
              Shell:\> ipsecconfig -p SAD -a --spi 0x3000 --local 2000:aaaa::1 --remote 2000:aaaa::2
                       --ipsec-proto ESP --encrypt-algo 3DESCBC --encrypt-key "3descbcin0100000" --auth-algo SHA1HMAC
                       --auth-key  "sha1in01"

    Example4: Set a PAD entry
              Shell:\> ipsecconfig -p PAD -a --peer-address 2000:aaaa::2 --auth-proto IKEv2 --auth-method PreSharedSecret
                       --auth-data "Hello World"

========================================================================================================================
                                                   ISCSI CONFIGURATION
========================================================================================================================
To provide a user-friendly configuration environment, the iSCSI driver supports UI configuration within
UEFI setup browser->Device Manager->iSCSI Configuration. To configure a workable iSCSI initiator, the user first needs
to set an iSCSI initiator name, which is required in IQN format defined by RFC3720. Then the user should select
"Add an Attempt" to fill in iSCSI session parameters for one or more attempted iSCSI paths. Users may select
"Delete Attempts" or "Change Attempt Order" to operate the added attempts.
1.  Add an Attempt
    1)  When "Add an Attempt" is selected, a page pops up for selecting the particular network card for the new adding
        iSCSI attempting path. The Media Access Control(MAC) address and PCI Function Address (PFA) info are provided to
        facilitate the selection.
    2)  After a network card is selected by entering "Port **:**:**:**:**:**", the user goes to the "Attempt Configuration"
        page, which is used to configure iSCSI session parameters. The iSCSI session parameter are follows:

        Parameters                   Options            Description
        -------------------------------------------------------------------------------------------------------
        iSCSI Attempt Name                              A meaningful and readable name for the new attempt.

        iSCSI Mode                                      The mode state of the new attempt.
                                     Disabled           This attempt path is created but disabled.
                                     Enabled            This attempt path is created for single
                                                        path mode. In single path mode, only the
                                                        successful established iSCSI attempts will
                                                        be recorded in iBFT.
                                     Enabled for MPIO   This attempt path is created for multipath
                                                        I/O mode. In this mode, the first connected
                                                        attempt will be recorded in iBFT with "Firmware
                                                        boot selected" flag in entry 0, other attempts
                                                        will also be recorded accordingly.

        Internet Protocol                               The under layer stack, IPv4 or IPv6.
                                     IP4                This attempt path will try to establish iSCSI
                                                        connection by IPv4 stack.
                                     IP6                This attempt path will try to establish iSCSI
                                                        connection by IPv6 stack.
                                     Autoconfigure      This attempt path will try to establish iSCSI
                                                        connection by requesting DHCP server. If DHCPv4
                                                        server offers a valid iSCSI configuration, this
                                                        attempt will use IPv4 stack, if DHCPv6 server
                                                        offers use IPv6 stack.

        Connection Retry Count                          The number of retry times of iSCSI session login

        Connection Establishing Timeout                 The timer of iSCSI session login, in milliseconds.
                                                        The minimum value is 100 milliseconds and the
                                                        maximum is 20 seconds.

        Enable DHCP                                     Whether to request DHCP server for IP address
                                     Get target         Whether to request DHCP server for iSCSI target info.
                                     info via DHCP      This parameter could be configured only when "Enable
                                                        DHCP" is set.

        Initiator IP Address                            Configure the IP address of iSCSI initiator statically
                                                        when DHCP is not enabled

        Initiator Subnet Mask                           Configure the subnet mask of iSCSI initiator statically

        Gateway                                         Configure the gateway for iSCSI initiator statically

        Target Name                                     The iSCSI target name

        Target IP Address                               The IP address of iSCSI target

        Target Port                                     The iSCSI target port

        Boot LUN                                        The logical unit number of iSCSI target

        Authentication Type                             The authentication method of iSCSI login

        CHAP                                            The default authentication method for iSCSI login,
                                                        Challenge Handshake Authentication Protocol defined in RFC1994

        None                                            No authentication


        Note: "Initiator IP Address", "Initiator Subnet Mask" and "Gateway" can be configured only when "Enable DHCP"
              is not set and Internet Protocol is IP4.
        Note: "Target Name", "Target IP Address", "Target Port", "Boot LUN" can be configured only when "Get target info via DHCP"
              is not set.

2.  To delete one or more existing attempts, use the "Esc" key to go back to main page. Then select "Delete Attempts".
    For instance, to delete "Attempt 2", select "Attempt 2" by pressing the "Enter" key. Then select "Commit Changes
    and Exit" and press "Enter".

3.  To change current order of existing attempts, use the "Esc" key to go back to main page. Then select "Change Attempt Order".
    For instance, to move Attempt 2 ahead, first select "Attempt 2", then use the "+" key to move it up and press "Enter".
    Then select "Commit Changes and Exit" and press "Enter".

* Other names and brands may be claimed as the property of others.
