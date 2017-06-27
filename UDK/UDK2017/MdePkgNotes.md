# [UDK2017]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2017) MdePkg Notes



##  NEW FEATURES AND CHANGES
1.  Add Unified Extensible Firmware(UEFI) specification 2.6 defined Protocols/PPIs/GUIDs and their related data
    structure definitions.
    1)  Protocols:
        EFI_ERASE_BLOCK_PROTOCOL
        EFI_HII_IMAGE_DECODER_PROTOCOL
        EFI_HII_IMAGE_EX_PROTOCOL
        EFI_RAM_DISK_PROTOCOL
        EFI_SD_MMC_PASS_THRU_PROTOCOL
        EFI_SHELL_PROTOCOL
        EFI_SHELL_DYNAMIC_COMMAND_PROTOCOL
        EFI_SHELL_PARAMETERS_PROTOCOL
        EFI_SUPPLICANT_PROTOCOL
        EFI_TLS_PROTOCOL
        EFI_TLS_CONFIGURATION_PROTOCOL
        EFI_WIRELESS_MAC_CONNECTION_II_PROTOCOL
    2)  GUIDs:
        EFI_MEMORY_ATTRIBUTES_TABLE_GUID
        EFI_ERROR_SECTION_PROCESSOR_SPECIFIC_IA32X64_GUID
    3)  Miscellaneous UEFI 2.6 elements:
        a) Add EMMC_DEVICE_PATH, device path
        b) Move EBC opcode related definitions
        c) Add new HII action type EFI_BROWSER_ACTION_SUBMITTED
        d) Add new HII block EFI_HII_IIBT_PNG_BLOCK, EFI_HII_GIBT_VARIABILITY_BLOCK
        e) Add EFI_OS_INDICATIONS_START_PLATFORM_RECOVERY definition
        f) Add new enum EfiPlatformConfigurationActionUnsupportedGuid
        g) Correct EFI_MTFTP6_ERRORCODE_ILLEGAL_OPERATION value from 6 to 4
        h) Add new error status EFI_HTTP_ERROR, EFI_WARN_FILE_SYSTEM
        i) Add new traffic statistics definition for Wireless NIC

2.  Add Platform Initialization(PI) specification 1.4a defined Protocols/PPIs/GUIDs and their related data
    structure definitions.
    1)  Update EFI_RESOURCE_ATTRIBUTE_READ_ONLY_PROTECTABLE to 0x00080000
    2)  Remove the fix artificial limitation of SkuId range in PCD SetSkuId()
    3)  Add PI1.5 EFI_PEI_GRAPHICS_DEVICE_INFO_HOB

3.  Add/Upgrade Industry standard definitions.
    1)  Add EFI_SMM_COMMUNICATION_ACPI_TABLE_2 to include invocation register support
    2)  Add ACPI6.1 tables
    3)  Update ACPI6.0 tables
        a) Update MADT Revision
        b) Add GIC version
        c) Add Hardware Error Notification types
        d) Correct I/O Remapping Table signature definition
    4)  Update ACPI5.1 tables
        a) Add GIC version
    5)  Add DMA Remapping Reporting (DMAR) ACPI table definition
    6)  Add ACPI Low Power Idle Table (LPIT) definitions
    7)  Add additional Atapi definitions
    8)  Add Windows SMM Security Mitigation Table
    9)  Add DHCPv4 and DHCPv6 definitions
    10) Add SD/EMMC common definitions
    11) Add HTTP 1.1 industry standard definitions
    12) Add Transport Layer Security  -- TLS 1.0/1.1/1.2 Standard definitions
    13) Add Ipmi2.0 definitions
    14) Add NVMe v1.1 definitions.
    15) Update PCI/PCIE definition
        a) Add EFI_PCI_CAPABILITY_ID_SHPC
        b) Update PCIe Extended Capabilities definition
        c) Remove out-of-Spec IncompatiblePciDevice macros
    16) Add PCI Express 3.1 definition
    17) Add Unmap command support in SCSI
    18) Add DDR3, DDR4 and LPDDR definition
    19) Update SMBIOS definition
        a) Add missing SMBIOS definitions for SATA and SAS Ports
        b) Add new defines for SMBIOS record type 43
        c) Add definitions for SMBIOS spec 3.1.0 and 3.1.1
    20) Add TCG Storage Core and Opal definitions
    21) Update TPM2 ACPI Table revision to 4
    22) Add TPM TIS definition
    23) Add TPM PTP definition
    24) Update TCG EFI Platform Definition
        a) Define Startup Locality Event & Indicator
        b) Add TCG_PCR_EVENT2_HDR definition
        c) Add UEFI_VARIABLE_DATA
    25) Update TPM2 to add 3 new TPM_RH Constants

4.  Add PCD PcdPort80DataWidth and PcdUartDefaultReceiveFifoDepth.

5.  Add new Library classes:
    1)  SmmIoLib
    2)  RngLib
    3)  SmiHandlerProfileLib

6.  Add new library APIs in existing library classes:
    1)  Add StrnSizeS(), StrDecimalToUintnS(), StrDecimalToUint64S(), StrHexToUintnS(), StrHexToUint64S(),
        AsciiStrnSizeS(), AsciiStrDecimalToUintnS(), AsciiStrDecimalToUint64S(), AsciiStrHexToUintnS(), AsciiStrHexToUint64S(),
        StrToIpv6Address(), StrToIpv4Address(), StrToGuid(), StrHexToBytes(),
        AsciiStrToIpv6Address(), AsciiStrToIpv4Address(), AsciiStrToGuid(), AsciiStrHexToBytes(),
        UnicodeStrToAsciiStrS(), UnicodeStrnToAsciiStrS(), AsciiStrToUnicodeStrS(), AsciiStrnToUnicodeStrS() in BaseLib.
    2)  Deprecate AsciiStrToUnicodeStr(), UnicodeStrToAsciiStr() in BaseLib
    3)  Add IsZeroGuid() and IsZeroBuffer() in BaseMemoryLib
    4)  Add ASSERT_RETURN_ERROR() in DebugLib
    5)  Add GetFileDevicePathFromAnyFv() in DxeServicesLib
    6)  Add IoReadFifo'`[8|16|32]'`(), IoWriteFifo'`[8|16|32]'`() in IoLib
    7)  Add PeCoffSearchImageBase() in PeCoffGetEntryPointLib
    8)  Add SerialPortSetControl(), SerialPortGetControl(), SerialPortSetAttributes() in SerialPortLib
    9)  Add EfiEventGroupSignal(), EfiEventEmptyFunction() in UefiLib
    10) Add ScsiRead10CommandEx(), ScsiWrite10CommandEx(), ScsiRead16CommandEx(), ScsiWrite16CommandEx() in UefiScsiLib
    11) Add UnicodeValueToStringS(), AsciiValueToStringS() in PrintLib
    12) Deprecate UnicodeValueToString(), AsciiValueToString() in PrintLib

7.  Add new library instances:
    1)  BasePciSegmentLibPci
    2)  UefiDebugLibDebugPortProtocol
    3)  SmmIoLib
    4)  BaseRngLib
    5)  SmmPciExpressLib
    6)  SmiHandlerProfileLibNull

8.  Update Base definition
    1)  Add IPv4_ADDRESS and IPv6_ADDRESS
    1)  Introduce the ARRAY_SIZE() function-like macro
    1)  Add NORETURN attribute and UNREACHABLE() macro.
    1)  Enable new MS VA intrinsics for GNUC x86 64bits build
    1)  Retire NO_BUILTIN_VA_FUNCS macro
    1)  Move CHAR_NULL definition
    1)  Add enumeration size checks
    1)  Add page allocation granularity

9.  Disable the C4701 and C4703 warnings for VS2015

10. Add IMAGE_TOKEN() macro to access image resource in C and VFR

11. Add NASM source files

12. Convert all .uni files to utf-8



##  BUG FIXES
1.  Update ASSERT() macro as unreachable for analyzers
2.  Use gEfiCallerBaseName in DebugAssert to display the module name
3.  Update S3BootScriptSaveMemPoll() to accept 64-bit LoopTimes
4.  Update BaseSynchronizationLib
    1) Add volatile to Interlocked*() APIs
    2) Do not check timeout if lock released
    c) Add spin lock alignment for IA32/x64
5.  Update UefiScsiLib
    1) Close event when SCSI command fails
    2) Raise the Tpl of async IO callback to TPL_NOTIFY
6.  Update BaseLib<br>
    1) Don't rely on undefined behavior in arithmetic shift in RShiftU64()<br>
    2) Enhance PathRemoveLastItem() to support "FS0:File.txt"<br>
    3) Update PathRemoveLastItem to handle root paths properly<br>
    4) Fix PathCleanUpDirectories to correctly handle "\.\"<br>
    5) Fix PathCleanUpDirectories to correctly handle "..\.."<br>
7.  Enhance SmmIsBufferOutsideSmmValid() check for fixed comm buffer.
8.  Reinitialize twice-iterated VA_LIST in variadic function UefiDevicePathLibCatPrint()
9.  Update UefiDevicePathLib<br>
    1) Validate buffer length before use buffer<br>
    2) Fix the wrong MAC address length<br>
    3) Fix FromText bug for multi-instance device path<br>
    4) Reverse the byte order of BD_ADDR for Bluetooth<br>
10. Check FV alignment when building FV HOB
11. Make GetHobList working before Constructor is called
12. Handle potential NULL FvHandle in GetSectionFromFv ()
13. Make sure FvInfo has FFS2 format if NULL FvFormat in PeiServicesInstallFvInfoPpi()
14. Update BaseMemoryLib
    1) Check for zero length in ZeroMem ()
    2) Wide aligned accesses to 32 or 64 bits

##  KNOWN ISSUES
1.  EFI_SIZE_TO_PAGES() macro requires that the input parameter is UINTN data type.
2.  SecPeiDxeTimerLibCpu library instance can be used by drivers of type DXE_RUNTIME_DRIVER and DXE_SMM_DRIVER in
    their initialization without any issues. But, those driver needs be careful in the implementation
    of runtime services and SMI handlers.
3.  BaseExtractGuidedSectionLib library instance can be used by drivers of type DXE_RUNTIME_DRIVER and DXE_SMM_DRIVER in
    their initialization without any issues.

    
