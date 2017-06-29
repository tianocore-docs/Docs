================================================================================
                    HOW TO ENABLE SECURE BOOT SERVICE
================================================================================
Based on original variable driver in MdeModulePkg, variable driver in SecurityPkg
provides authenticated variable service in UEFI 2.3.1 spec. Runtime crypto library,
OpenSSL* library and variable driver are required to enable this feature.

1.  Ensure OpensslLib* library instance is defined in [LibraryClasses] section of
    the platform DSC file:
    IntrinsicLib|CryptoPkg/Library/IntrinsicLib/IntrinsicLib.inf
    OpensslLib|CryptoPkg/Library/OpensslLib/OpensslLib.inf

2.  Ensure BaseCryptLib library instances are defined in the platform DSC file:
    For PEI driver: BaseCryptLib|CryptoPkg/Library/BaseCryptLib/PeiCryptLib.inf
    For DXE driver: BaseCryptLib|CryptoPkg/Library/BaseCryptLib/BaseCryptLib.inf
    For RUNTIME driver: BaseCryptLib|CryptoPkg/Library/BaseCryptLib/RuntimeCryptLib.inf
    For SMM driver: BaseCryptLib|CryptoPkg/Library/BaseCryptLib/SmmCryptLib.inf

3.  Ensure platform secure library is added in platform DSC. A NULL instance for
    PlatformSecureLib is provided as below. It can be replaced by a platform-specific
    library instance.
    SecurityPkg/Library/PlatformSecureLibNull/PlatformSecureLibNull.inf

4.  Ensure library instance DxeImageVerificationLib is added to LibraryClasses section
    of module SecurityStubDxe in the platform DSC file:
    MdeModulePkg/Universal/SecurityStubDxe/SecurityStubDxe.inf {
       <LibraryClasses>
          NULL|SecurityPkg/Library/DxeImageVerificationLib/DxeImageVerificationLib.inf
    }

5.  Add Authenticated Variable driver INF to [Component] section of the platform
    DSC file:
    SecurityPkg/VariableAuthenticated/Pei/VariablePei.inf
    SecurityPkg/VariableAuthenticated/RuntimeDxe/VariableRuntimeDxe.inf
    SecurityPkg/VariableAuthenticated/SecureBootConfigDxe/SecureBootConfigDxe.inf

6.  Remove original variable driver INF from the platform FDF file:
    INF MdeModulePkg/Universal/Variable/Pei/VariablePei.inf
    INF MdeModulePkg/Universal/Variable/RuntimeDxe/VariableRuntimeDxe.inf

    Add Authenticated Variable Driver INF to the platform FDF file:
    INF SecurityPkg/VariableAuthenticated/Pei/VariablePei.inf
    INF SecurityPkg/VariableAuthenticated/RuntimeDxe/VariableRuntimeDxe.inf
    INF SecurityPkg/VariableAuthenticated/SecureBootConfigDxe/SecureBootConfigDxe.inf

7.  Update Variable GUID value of VARIABLE_STORE_HEADER in FDF file as:
    #Signature: gEfiAuthenticatedVariableGuid =
    # {0xaaf32c78, 0x947b, 0x439a, { 0xa1, 0x80, 0x2e, 0x14, 0x4e, 0xc3, 0x77, 0x92}}
    0x78, 0x2c, 0xf3, 0xaa, 0x7b, 0x94, 0x9a, 0x43,
    0xa1, 0x80, 0x2e, 0x14, 0x4e, 0xc3, 0x77, 0x92,

8.  Set appropriate value of gEfiMdeModulePkgTokenSpaceGuid.PcdMaxVariableSize
    for security feature relative databases which uses EFI Variable as storage.
    Each database stores in a single variable, the maximum variable size is
    defined by PCD value of gEfiMdeModulePkgTokenSpaceGuid.PcdMaxVariableSize.
    Database categories include:
    1)  PK database: only one entry for public key of PK plus header info.
    2)  KEK database: multi-entry for public key of KEK plus header info.
    3)  Authorized signature database: multi-entries for authorized signatures
        and one entry for root X509 certificate, plus header info.
    4)  Forbidden signature database: multi-entries for forbidden signatures,
        plus header info.

    NOTICE: Typically the size of one X509 certificate is ~2k, which may exceed
            the default maximum variable size. Please adjust the value by PCD if
            needed.

9.  Set a platform policy of image verification by PCDs.
    User can customize platform policy of image verification by PCD value
    before build a platform. In [PcdsFixedAtBuild] section of SecurityPkg.dec
    file, set the PCD value for each type of device accordingly.

    For example, if the platform policy is defined as:
    1)  Trust all images from OptionROM.
    2)  Validate all images from removable devices and deny execute when security
        violation occurs.
    3)  Validate all images from hard disk and query user to make decision when
        security violation occurs.

    In this case, the PCD value should be set as following:
    gEfiSecurityPkgTokenSpaceGuid.PcdOptionRomImageVerificationPolicy|0x00|UINT32|0x00000001
    gEfiSecurityPkgTokenSpaceGuid.PcdRemovableMediaImageVerificationPolicy|0x04|UINT32|0x00000002
    gEfiSecurityPkgTokenSpaceGuid.PcdFixedMediaImageVerificationPolicy|0x05|UINT32|0x00000003

10. Another authenticated variable service, named SMM authenticated variable, is
    also provided in SecurityPkg. SMM authenticated variable driver requires SMM
    FVB protocol which should be provided by platform driver and SMM FTW protocol
    which is already provided in MdeModulePkg. To enable SMM authenticated variable
    driver instead of non-SMM authenticated variable driver in SecurityPkg,
        SecurityPkg/VariableAuthenticated/RuntimeDxe/VariableRuntimeDxe.inf
    should be replaced by following drivers in step 5 and 6:
        SecurityPkg/VariableAuthenticated/RuntimeDxe/VariableSmm.inf
        SecurityPkg/VariableAuthenticated/RuntimeDxe/VariableSmmRuntimeDxe.inf

================================================================================
                        HOW TO ENABLE USER IDENTIFICATION
================================================================================
In UID (User Identification) infrastructure, there are 4 UEFI drivers, one library
instance and some platform specific changes in BDS. To enable UID feature:
1.  Ensure the platform specific code had been integrated into the platform BDS.
    Identify () in User Manager Protocol should be invoked after console is ready
    and authentication device (e.g. Smart card) is connected.

2.  Ensure OpensslLib* library instance is defined in [LibraryClasses] section of
    the platform DSC file:
    IntrinsicLib|CryptoPkg/Library/IntrinsicLib/IntrinsicLib.inf
    OpensslLib|CryptoPkg/Library/OpensslLib/OpensslLib.inf

3.  Ensure BaseCryptLib library instances in each phase are defined in the platform
    DSC file:
    PEI phase: BaseCryptLib|CryptoPkg/Library/BaseCryptLib/PeiCryptLib.inf
    DXE phase: BaseCryptLib|CryptoPkg/Library/BaseCryptLib/BaseCryptLib.inf
    RUNTIME phase: BaseCryptLib|CryptoPkg/Library/BaseCryptLib/RuntimeCryptLib.inf

4.  Add UID drivers to [Component] section of the platform DSC file:
    1)  UserIdentifyManagerDxe driver produces user manager protocol and loads
        deferred image after user authentication.
        SecurityPkg/UserIdentification/UserIdentifyManagerDxe/UserIdentifyManagerDxe.inf

    2)  PwdCredentialProviderDxe driver produces user credential protocol and
        provides support for password credential.
        SecurityPkg/UserIdentification/PwdCredentialProviderDxe/PwdCredentialProviderDxe.inf

    3)  UsbCredentialProviderDxe driver produces user credential protocol and
        provides support for secure card credential.
        SecurityPkg/UserIdentification/UsbCredentialProviderDxe/UsbCredentialProviderDxe.inf

    4)  UserProfileManagerDxe driver provide UI configure for user profiles in
        UEFI HII Form Browser. It is an sample driver to configure basic user
        information. For advanced configuration, such as forbidding a user to
        load image from USB disk, it is not supported by this driver.
        SecurityPkg/UserIdentification/UserProfileManagerDxe/UserProfileManagerDxe.inf

5.  Add library instance DxeDeferImageLoadLib to LibraryClasses section of module
    SecurityStubDxe in the platform DSC file.
    The library instance is invoked during loading an image into memory. The
    image loading could be deferred by the predefined policy in a PCD before user
    authentication, or be verified by current user access policy after user
    authentication.
    MdeModulePkg/Universal/SecurityStubDxe/SecurityStubDxe.inf {
       <LibraryClasses>
         NULL|SecurityPkg/Library/DxeDeferImageLoadLib/DxeDeferImageLoadLib.inf
    }

6.  Add UID drivers to the platform FDF file:
    INF  SecurityPkg/UserIdentification/UserIdentifyManagerDxe/UserIdentifyManagerDxe.inf
    INF  SecurityPkg/UserIdentification/UserProfileManagerDxe/UserProfileManagerDxe.inf
    INF  SecurityPkg/UserIdentification/PwdCredentialProviderDxe/PwdCredentialProviderDxe.inf
    INF  SecurityPkg/UserIdentification/UsbCredentialProviderDxe/UsbCredentialProviderDxe.inf

7.  Set the platform policy by PCDs.
    User can customize platform policy by changing the default PCD value in
    SecurityPkg.dec before building a platform.

    1)  Deferred image load policy
        The policy makes use of bitmasks for five predefined device types. If
        a bit is set, the image from the corresponding device will be trusted
        when loading. Image from any device is trusted by default.

    2)  USB token file name
        USB credential provider will read a file as the credential information.
        The token file should be at the root directory of USB storage disk and
        its name is specified by PCD value. "Token.bin" is the default name of
        token file.

================================================================================
                        HOW TO ENABLE TCG TPM
================================================================================
TCG measured boot consists of two PEI modules, four DXE drivers and three libraries
and some platform specific changes. To enable TCG TPM feature:

1.  Ensure the platform specific changes had been done.
    1)  Memory should be cleared if ClearMemory bit of variable MemoryOverwriteRequestControl
        is set when doing memory initialization.
    2)  TcgPhysicalPresenceLibProcessRequest () from TCG physical presenceLib library
        should be invoked to process pending TPM request in BDS when console is ready.

2.  Ensure OpensslLib* library instance is defined in [LibraryClasses] section of
    the platform DSC file:
    IntrinsicLib|CryptoPkg/Library/IntrinsicLib/IntrinsicLib.inf
    OpensslLib|CryptoPkg/Library/OpensslLib/OpensslLib.inf

3.  Ensure BaseCryptLib library instances in each phase are defined in the platfrom
    DSC file:
    PEI phase: BaseCryptLib|CryptoPkg/Library/BaseCryptLib/PeiCryptLib.inf
    DXE phase: BaseCryptLib|CryptoPkg/Library/BaseCryptLib/BaseCryptLib.inf

4.  Add library instance in the platform DSC file.
    1)  TPM common library
        It provides some common function routines used by TCG drivers.
        SecurityPkg/Library/TpmCommLib/TpmCommLib.inf
    2)  TCG physical presence library
        This library requires output device to display TPM state change request and
        input device to get user confirmation. Often it is invoked by BDS driver when
        console is ready.
        SecurityPkg/Library/DxeTcgPhysicalPresenceLib/DxeTcgPhysicalPresenceLib.inf
    3)  TPM measure boot library
        The library instance provides measurement and log service for TPM measured
        boot. The instance is invoked during loading an image into memory. It should
        be added into LibraryClasses section of module SecurityStubDxe in the platform
        DSC file.
        SecurityPkg/Library/DxeTpmMeasureBootLib/DxeTpmMeasureBootLib.inf
        MdeModulePkg/Universal/SecurityStubDxe/SecurityStubDxe.inf {
          <LibraryClasses>
            NULL|SecurityPkg/Library/DxeTpmMeasureBootLib/DxeTpmMeasureBootLib.inf
        }

5.  Add TPM drivers to [Component] section of the platform DSC file:
    1)  TCG TPM PEI driver initializes TPM device and measures the drivers in firmware.
        SecurityPkg/Tcg/TcgPei/TcgPei.inf
    2)  TCG TPM DXE driver produces EFI TCG protocol and measure the drivers which
        are not from firmware.
        SecurityPkg/Tcg/TcgDxe/TcgDxe.inf
    3)  TCG SMM driver implements TPM definition block in ACPI table and registers
        SMI callback functions for physical presence and MemoryClear to handle the
        requests from ACPI method.
        SecurityPkg/Tcg/TcgSmm/TcgSmm.inf
    4)  TCG UI driver provides a generic TCG configuration page in setup browser to
        config TCG items.
        SecurityPkg/Tcg/TcgConfigDxe/TcgConfigDxe.inf
    5)  TCG physical presence PEI driver produces PEI_LOCK_PHYSICAL_PRESENCE_PPI to
        indicate whether TPM need be locked in PEI phase or not. It can be replaced
        by a platform specific driver.
        SecurityPkg/Tcg/PhysicalPresencePei/PhysicalPresencePei.inf
    6)  TCG memory overwrite Control driver initilizes MemoryOverwriteRequestControl
        variable. It will clear MOR_CLEAR_MEMORY_BIT bit if it is set.
        SecurityPkg/Tcg/MemoryOverwriteControl/TcgMor.inf

6.  Add TPM drivers to the platform FDF file:
    INF  SecurityPkg/Tcg/TcgPei/TcgPei.inf
    INF  SecurityPkg/Tcg/TcgDxe/TcgDxe.inf
    INF  SecurityPkg/Tcg/TcgSmm/TcgSmm.inf
    INF  SecurityPkg/Tcg/TcgConfigDxe/TcgConfigDxe.inf
    INF  SecurityPkg/Tcg/PhysicalPresencePei/PhysicalPresencePei.inf
    INF  SecurityPkg/Tcg/MemoryOverwriteControl/TcgMor.inf

7.  Set the platform policy by PCDs.
    User can customize platform policy by changing the default PCD value in
    SecurityPkg.dec before building a platform.

    1)  TCG platform type
        PCD PcdTpmPlatformClass specifies the type of TCG platform that contains
        TPM chip. Its value is set to 0 for PC client type by default. It should
        be set 1 for server.
        gEfiSecurityPkgTokenSpaceGuid.PcdTpmPlatformClass|0|UINT8|0x00000006

    2)  Hide TPM device
        The TPM device can be hided from firmware and OS. PcdHideTpm can dynamically
        control whether to hide the TPM if PcdHideTpmSupport is set TRUE.
        gEfiSecurityPkgTokenSpaceGuid.PcdHideTpmSupport|FALSE|BOOLEAN|0x00000007

================================================================================
                                 NOTES
================================================================================
1.  In this version of implementation of authenticated variable service, we support:
    1)  Public exponent of RSA key value is fixed as 0x10001.
    2)  Encoding schema of RSA is PKCS1.15.
2.  Currently certificate time expiration checking is ignored.
3.  No real-time CRL checking requirements for performance and size restriction in
    pre-boot environment.
4.  Variable Size Limitation: KEK/X509/Signature Database store as authenticated
    variables in the system,  with the database size limitation of max variable
    size of the platform. Users may choose to increase max variable size by PCD
    or to delete unused items when the database is full.

* Other names and brands may be claimed as the property of others.
