# [UDK2017]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2017) MdeModulePkg Notes
##  NEW FEATURES AND CHANGES
1.  PeiCore: Allow DxeIpl to load without permanent memory.
2.  DxeCore:
    1)  Add MemoryAttributesTable support.
    2)  Add UEFI image protection and NX memory protection, and add PCDs PcdImageProtectionPolicy and
        PcdDxeNxMemoryProtectionPolicy to set policy.
    3)  Switch to MdePkg allocation granularity macros.
3.  PiSmmCore:
    1)  Add SmmMemoryAttributesTable support.
    2)  Enhance SMM FreePool to catch buffer overflow.
    3)  Switch to MdePkg allocation granularity macros.
4.  DxeIplPeim:
    1)  Enable dynamism for PCD PcdSetNxForStack.
    2)  Implement non-exec stack for ARM/AARCH64.
    3)  Allow DxeIpl to load without permanent memory.
    4)  Switch to separate ArmMmuLib.
    5)  Display new stack base and size.
    6)  Add support for PCD PcdPteMemoryEncryptionAddressOrMask.
5.  Capsule and Recovery:
    1)  Add ProcessCapsules() API in library class CapsuleLib and instance DxeCapsuleLibNull.
    2)  Add CapsuleLib library instance DxeCapsuleLibFmp to handle Microsoft UX capsule, UEFI defined FMP capsule.
    3)  Add library class FmpAuthenticationLib and instance FmpAuthenticationLibNull.
    4)  Add CapsuleApp application that can help perform capsule update, and dump capsule information,
        capsule status variable, ESRT and FMP in UEFI shell.
    5)  Add PCDs.<br>
        PcdRecoveryFileName<br>
        PcdStatusCodeSubClassCapsule<br>
        PcdCapsuleStatusCodeProcessCapsulesBegin<br>
        PcdCapsuleStatusCodeProcessCapsulesEnd<br>
        PcdCapsuleStatusCodeUpdatingFirmware<br>
        PcdCapsuleStatusCodeUpdateFirmwareSuccess<br>
        PcdCapsuleStatusCodeUpdateFirmwareFailed<br>
        PcdCapsuleStatusCodeResettingSystem<br>
        PcdSystemFmpCapsuleImageTypeIdGuid<br>
        PcdTestKeyUsed<br>
    6)  Use PcdRecoveryFileName PCD in CdExpressPei.
    7)  CapsulePei
        1)  Display new stack base and size.
        2)  Add support for PCD PcdPteMemoryEncryptionAddressOrMask.
6.  Add SMI handler profile feature, the wiki page at
    https://github.com/tianocore/tianocore.github.io/wiki/SMI-handler-profile-feature
    introduces how to enable the feature.
    1)  Add SmiHandlerProfile.h to define SMI handler profile data structure and protocol.
    2)  Add PCD PcdSmiHandlerProfilePropertyMask to control SmiHandlerProfile behavior.
    3)  Add SmiHandlerProfile support in PiSmmCore to manage SMI handler profile.
    4)  Add SmiHandlerProfileInfo application to dump SMI handler profile in UEFI shell.
    5)  Add SmiHandlerProfileLib library instance SmmSmiHandlerProfileLib to be linked by SmmChildDispatcher
        to register/unregister SMI handler.
7.  Variable DXE/SMM driver:
    1)  Enhance performance by reading from existed memory cache.
    2)  Add MorLock support for the secure MOR implementation published by Microsoft at
        https://msdn.microsoft.com/en-us/library/windows/hardware/mt270973(v=vs.85).aspx.
    3)  Use UEFI_VARIABLE_DATA data structure according to TCG PC-Client PFP Spec 00.21.
    4)  Update PCR[7] measurement for new TCG spec TCG PC Client PFP 00.37.
8.  PCD:
    1)  Follow PI1.4a to fix artificial limitation of SkuId range.
    2)  Update PCD database structure definition to match BaseTools.
9.  Enhance memory profile for memory leak detection, the wiki page at
    https://github.com/tianocore/tianocore.github.io/wiki/Memory-leak-detection-with-memory-profile-feature
    introduces how to enable the feature.
    1)  Extend memory profile definitions for memory leak detection in MemoryProfile.h.
    2)  Extend PCD PcdMemoryProfilePropertyMask and add PCD PcdMemoryProfilePropertyMask for memory leak detection
        by memory profile.
    3)  Enhance memory profile management for memory leak detection in DxeCore and PiSmmCore.
    4)  Enhance output info for memory leak detection by memory profile in MemoryProfileInfo application.
    5)  Add library class MemoryProfileLib and instances UefiMemoryAllocationProfileLib,
        SmmMemoryAllocationProfileLib, DxeCoreMemoryAllocationProfileLib and PiSmmCoreMemoryAllocationProfileLib
        to provide services to record memory profile of multilevel caller.
10. ACPI:
    1)  Add clarification for PcdAcpiDefault value PCD.
    2)  Move S3Ready() functional code from AcpiS3SaveDxe in IntelFrameworkModulePkg to S3SaveStateDxe,
        and also move PCD PcdS3BootScriptStackSize from IntelFrameworkModulePkg.
    3)  Add PCD PcdAcpiExposedTableVersions containing a bitmask describing which ACPI versions are targeted,
        and make 4 GB table allocation limit optional in AcpiTableDxe.
    4)  Add PCD PcdAcpiS3Enable and consume it in S3SaveStatedxe, SmmS3SaveStateDxe and BootScriptExecutorDxe.
    5)  Add support for PCD PcdPteMemoryEncryptionAddressOrMask in BootScriptExecutorDxe.
    6)  Support multiple PCI segment in PiDxeS3BootScriptLib.
11. Add EDKII SD/MMC stack, the stack includes:
    1)  DXE phase
        1)  SdMmcPciHcDxe driver to consume PciIo and produce SdMmcPassThru.
        2)  SdDxe driver to consume SdMmcPassThru to produce BlkIo1/BlkIo2.
        3)  EmmcDxe driver to consume SdMmcPassThru to produce BlkIo1/BlkIo2/SSP.
    2)  PEI phase
        1)  SdMmcPciHcPei driver to produce SdMmcHostController Ppi.
        2)  SdBlockIoPei driver to consume SdMmcHostController Ppi and produce VirutalBlkIo1&2.
        3)  EmmcBlockIoPei driver to consume SdMmcHostController Ppi and produce VirutalBlkIo1&2.
12. ATA:
    1)  Add generic SataController driver.
    2)  Use the new (incompatible) PortMultiplierPort semantics in AtaAtapiPassThruDxe and AtaBusDxe.
    3)  Enable 64-bit PCI DMA if the controller supports 64-bit DMA addressing in AtaAtapiPassThruDxe.
13. ScsiDiskDxe
    1)  Add BlockIO2 Support.
    2)  Add Erase Block Protocol support for UFS devices.
14. ISA:
    1)  Add Ps2KeyboardDxe to produce SimpleTextIn(Ex) protocol based on SIO protocol.
    2)  Add Ps2MouseDxe to produce SimplePointer protocol based on SIO protocol.
    3)  Move Ps2Policy protocol, PCD PcdFastPS2Detection, PcdPs2KbdExtendedVerification
        and PcdPs2MouseExtendedVerification from IntelFrameworkModulePkg.
15. EhciDxe and XhciDxe: Enable 64-bit PCI DMA if the controller supports 64-bit DMA addressing.
16. UfsPassThruDxe: Add Non-blocking I/O Support.
17. PCI:
    1)  PciBusDxe: Add PCD PcdPciDegradeResourceForOptionRom and make OPROM BAR degradation configurable.
    2)  Non-discoverable device
        1)  Add non-discoverable device protocol.
        2)  Add helper library class and library instance NonDiscoverableDeviceRegistrationLib to register
            non-discoverable devices.
        2)  Add generic PCI I/O driver for non-discoverable devices.
    3)  PciHostBridge:
        1)  Add library class PciHostBridgeLib and instance PciHostBridgeLibNull to provide the platform specific
            information about the PCI Host Bridge.
        2)  Add generic PciHostBridgeDxe driver that links to PciHostBridgeLib provided by platform/silicon
            to produce PciRootBridgeIo and PciHostBridgeResourceAllocation protocols.
18. NvmExpressDxe:
    1)  Add BlockIo2 support.
    2)  Move/Replace NvmExpressHci.h definitions to Nvme.h.
    3)  Enable 64-bit PCI DMA if the controller supports 64-bit DMA addressing.
19. Add RamDiskDxe to produce the EFI RAM Disk Protocol, install RAM disk device path and block I/O protocols
    on the RAM disk device handle, and install RAM disk configuration form to HII database.
20. Add GraphicsOutputDxe to use the GraphicsInfo HOB and GraphicsDeviceInfo HOB passed from PEI to find
    the graphics controller to manage and produce the GraphicsOutput protocol.
21. ConsplitterDxe: Support toggle state sync between multiple keyboards.
22. TerminalDxe: Execute key notify func at TPL_CALLBACK to follow UEFI spec.
23. BdsDxe: Enable PlatformRecovery.
24. UefiBootManagerLib
    1)  Make the BmFindLoadOption function public.
    2)  Add Platform recovery support.
    3)  Support short-form URI boot to follow UEFI Spec.
    4)  Support booting from a remote file system exposed by a HTTP boot option.
    5)  Support LoadFile Protocol based on FV.
    6)  Refine the BmIsValidLoadOptionVariableName function to allow public use.
    7)  Expose EfiBootManagerGetLoadOptionBuffer() API.
    8)  Add EfiBootManagerDispatchDeferredImages API to dispatch the deferred images that are returned
        from all DeferredImageLoad instances.
    9)  Generate boot description for NVME.
25. Add RegularExpressionDxe to produce EFI_REGULAR_EXPRESSION_PROTOCOL based on Oniguruma v5.9.6(BSD 2-clause license),
    which provides full Unicode support, and POSIX ERE and Perl regex syntaxes.
26. Add LoadFileOnFv2 to search APPLICATION in FV and install LoadFile protocol for every found one.
27. Add Platform Logo protocol, add LogoDxe that embeds the image resource in PE resource section and then produces
    Platform Logo protocol which can return the images in pixel format.
28. Add library class and instance BootLogoLib to provide interfaces about logo display, and the library is only
    intended to be used by PlatformBootManagerLib to show progress bar and logo.
29. LzmaCustomDecompressLib: Update LZMA to new 16.04 version.
30. Add Brotli algorithm decompression library.
31. Add library class and instance FrameBufferBltLib that provides interfaces to perform UEFI Graphics Output Protocol
    Video BLT operations.
32. Add DumpCpuContext() API in library class CpuExceptionHandlerLib and instance CpuExceptionHandlerLibNull.
33. Support EfiResetPlatformSpecific in ResetSystemRuntimeDxe and implement ResetPlatformSpecific
    in BaseResetSystemLibNull.
34. Add PciSioSerialDxe driver to manages UARTs on a SIO chip or a PCI/PCIE card, add PCD PcdSerialUseHalfHandshake
    to enable Serial device Half Hand Shake and PCD PcdPciSerialParameters to configure Pci Serial Parameters.
35. Upstream SerialDxe from EmbeddedPkg, the SerialDxe layers on top of a Serial Port Library instance
    to produce serial IO protocol.
36. BaseSerialPortLib16550: Implement Get(Set)Control/SetAttributes.
37. PrintLib and Print2(S) protocol:
    1)  Add EFI_PRINT2S_PROTOCOL and update PrintDxe to produce both Print2 and Print2S protocols.
    2)  Use EFI_PRINT2S_PROTOCOL and add safe print functions [A|U]ValueToStringS in DxePrintLibPrint2Protocol.
    3)  Add deprecated flag for APIs [A|U]ValueToString in DxePrintLibPrint2Protocol and PrintDxe.
38. PerformanceLib:
    1)  Define PERFORMANCE_PROPERTY, and install performance property configuration table in DxeCorePerformanceLib
        and SmmCorePerformanceLib for DP application to remove TimerLib dependency.
    2)  Add PCD PcdMaxPeiPerformanceLogEntries16 to increase the maximum number of PEI performance log entries
        in PeiPerformanceLib and DxeCorePerformanceLib.
39. Move Smbios measurement from SecurityPkg TCG driver to SmbiosManagementDxe.
40. SmbiosDxe:
    1)  Move Smbios table MAX length definition to Mde header file.
    2)  Use definition in IndustryStandard/Smbios.h.
    3)  Update PcdSmbiosVersion to 0x0301 for SMBIOS spec 3.1.0.
    4)  Update PcdSmbiosDocRev to 0x1 for SMBIOS spec 3.1.1
41. EBC:
    1)  Add AARCH64 EBC VM support in EbcDxe.
    2)  Add EBC Debugger.
    3)  Add EBC Debugger configuration application.
42. IPMI:
    1)  Add library class IpmiLib and IPMI Ppi/Protocol header files.
    2)  Add IpmiLib library instances BaseIpmiLibNull, PeiIpmiLibIpmiPpi, DxeIpmiLibIpmiProtocol
        and SmmIpmiLibSmmIpmiProtocol.
43. Expose the DXE memory status code table from StatusCodeHandler to MemoryStatusCode.h
    and save the address of DXE/SMM memory status code table to Dxe/SmmConfigurationTable.
44. PeiDxeDebugLibReportStatusCode: Use gEfiCallerBaseName in DebugAssert to display the module name.
45. HelloWorld: Add sample help information through PE resource section.
46. Move PlatformHasAcpiGuid from EmbeddedPkg.
47. Add NASM source files.
48. Convert all .uni files to utf-8.
49. Use IsZeroGuid API for zero GUID checking.
50. Rebase to ARRAY_SIZE().
51. Use EfiEventEmptyFunction from UefiLib.
52. NETWORK:
    1)  SNP
        1)  Update the SNP.GetStatus() to pass the DBsize to UNDI driver in order to get an array of recycled
            transmitted buffer address.
        2)  Update the media status check logic to support those UNDI drivers which only support media present
            check in UNDI GET_STATUS command but not in UNDI INITIALIZE command.
        3)  Update the MediaPresent flag check logic in SNP.Initialize() according to UEFI specification.
    2)  MNP: MNP driver is updated to recycle the TX buffer from SNP asynchronously, instead of using a while
        loop wait after each transmit command. This will improve the network transmit performance. The UNDI/SNP
        implementation must follow UEFI 2.6 Appendix E.4.16 to return the recycled transmitted buffer address
        in UNDI GetStatus command to make it work, otherwise an incorrect recycled buffer address will cause
        a DEBUG version MNP driver ASSERT.
    3)  IP4
        1)  Update all IP4 stack drivers (ARP, DHCP4, IP4, UDP4, TCP4, MTFTP4, iSCSI, PXE, NetLib) to support IPv4
            classless IP addressing.
        2)  Ip4Config2 protocol implementation is updated to request for DNS server info when the IP policy is set
            to DHCP.
        3)  Change the default IPv4 configuration policy to Ip4Config2PolicyStatic.
        4)  Ip4.Confugure() is updated to allow the upper layer modules to obtain a default address by setting
            UseDefaultAddress to TRUE when a station address has not been set to IP4 CONFIG2 protocol yet.
    4)  HttpLib: Add some new interfaces for HTTP message/header parsing.
    5)  NetLib:
        1)  Add DNS QType and QClass values definition in NetLib.h
        2)  Define a general function NetLibCreateDnsQName() to create DNS QName.
53. HII
    1)  SetupBrowserDxe: Add new HII action type EFI_BROWSER_ACTION_SUBMITTED support.
    2)  SetupBrowserDxe/HiiDatabase: Share default if some default value are not specified.
        Previous behavior is only for the situation that a question has default value but there are no default values
        for all supported default type. The change will choose the smallest default id from the existing defaults,
        and share its value to other default id which does not have default value.
    3)  HiiDataBaseDxe:
        1)  Make HII configuration settings available to OS runtime and add PCD PcdHiiOsRuntimeSupport.
            This feature is aimed to allow OS make use of the HII database during runtime. The contents of the
            HII Database is exported to a buffer. The pointer to the buffer is placed in the EFI System Configuration
            Table, where it can be retrieved by an OS application.
        2)  Support EfiVarStore to get AltCfg from Driver.
        3)  Update HiiImage to support PNG/JPEG and add HiiImageEx implementation.
    4)  DriverSampleDxe:
        1)  Add an example to hook EFI_BROWSER_ACTION_SUBMITTED action in callback function.
        2)  Add an example to get default value from callback function for orderedlist.
    5)  Split UiApp application to UiApp application, BootManagerUiLib, DeviceManagerUiLib, BootMaintenanceManagerUiLib
        and FileExplorerLib for good modularity.
    6)  Add FileExplorer protocol and FileExplorerDxe to produce FileExplorer protocol, and add FileExplorerLib library
        instance DxeFileExplorerProtocol that implements FileExplorerLib library class based on FileExplorer protocol.



##  BUG FIXES
1.  PeiCore:
    1)  Fix issue AuthenticationStatus is not propagated correctly.
    2)  Retry to process NOT_DISPATCHED FV in PEI dispatcher.
    3)  Fix PeiInstallPeiMemory improper ASSERT test on second call.
    4)  Fix indentation for Xcode -Wempty-body warning.
    5)  Fix the debug info of PEI temp heap length.
    6)  Update PeiCore dispatcher to handle PEIM and PEI_CORE only for PcdShadowPeimOnBoot.
    7)  Make sure FvInfo has FFS2 format if Ffs2Guid FvFormat.
    8)  Make SetPeiServicesTablePointer() early in EntryPoint.
    9)  Reset PeimNeedingDispatch when security violation status is returned for a PEIM as its matched extraction ppi
        may not be installed.
    10) Don't cache GUIDED section with AUTH_NOT_TESTED AuthenticationStatus.
    11) Allocate BootServicesCode memory for PE/COFF images.
    12) Honour minimal runtime allocation granularity.
    13) Avoid EFI_IMAGE_MACHINE_TYPE_SUPPORTED to check arch.
2.  DxeCore:
    1)  Remove event from protocol database only if registered.
    2)  Update print error level for RuntimeDriver alignment check.
    3)  Update DxeCore dispatcher to ignore PEI depex and treat SMM depex as invalid for FV.
    4)  Add missing change for OEM reserved memory type to free the list entry.
    5)  Fully initialize image context before passing it to PeCoffLoaderRelocateImageExtraAction().
    6)  Add address boundary check for Type AllocateAddress.
    7)  Call PeCoffExtraActionLib member after Constructor.
    8)  Avoid assertion in CoreLocateProtocol.
    9)  Update memory pool algorithm to use 128 bytes as the start size region for better performance
        when LinkList check is enabled in DEBUG tip.
    10) Return correct AuthStatus for FvReadFile.
    11) Show error message on unaligned FvImage issue.
    12) Fix ASSERT() from GCD DEBUG() messages with NULL BaseAddress.
    13) Clear RT attribute on SetCapabilities.
    14) Deal with allocations spanning several memmap entries.
    15) Add missing id-to-string mapping for AARCH64.
    16) Restore mCurrentImage on all paths.
    17) Fix issue to print GUID value %g without pointer.
3.  PiSmmIpl:
    1)  Check order of EndOfDxe and DxeSmmReadyToLock.
    2)  Replace BASE_4GB with MAX_ADDRESS.
    3)  Fill Smram range for SMM driver when LMFA enabled.
    4)  Get SmramBase from mLMFAConfigurationTable when LMFA enabled.
4.  PiSmmCore:
    1)  Remove confusing CopyMem() of SMM_ENTRY_CONTEXT.
    2)  Check InternalAllocPoolByIndex status before referring buffer.
    3)  Uninstall LoadedImage protocol if SMM driver returns error and is unloaded.
    4)  Install LoadedImage protocol for PiSmmCore.
    5)  Replace BASE_4GB with MAX_ADDRESS.
    6)  Cache CommunicationBuffer info before using it
    7)  Retrieve Smram base address from system configuration table when LMFA enabled.
    8)  Set correct ImageAddress into LoadedImage.
5.  DxeIplPeim: Fix UINTN used wrongly for EFI_PHYSICAL_ADDRESS.
6.  Capsule and Recovery:
    1)  Validate capsule integrity by memory resource hob in CapsulePei.
    2)  Add ESRT_FW_TYPE_SYSTEMFIRMWARE check in EsrtDxe.
    3)  Fix capsule size mismatch issue in CdExpressPei.
7.  Use PcdSet##S to replace PcdSet##.
8.  Replace [Ascii|Unicode]ValueToString with [Ascii|Unicode]ValueToStringS.
9.  Variable DXE/SMM driver:
    1)  Add missing VA_COPY to fix Xcode build failure.
    2)  Initialize ##VariableTotalSize to 0 first after reclaim is failed before operating it by +=.
    3)  Make VarErrFlag be consistent in NV flash and cache.
    4)  Handle ftw driver executes prior to variable driver.
    5)  Add a missing variable info record.
    6)  Return error for empty str VariableName to GetVariable.
    7)  Do not check CommBufferSize buffer.
    8)  Check InfoSize correctly for GET_STATISTICS.
10. VarCheck:
    1)  Handle the case that no property set to variable with wildcard name in VarCheckLib.
    2)  Add PlatformRecovery#### entry in VarCheckLib and VarCheckUefiLib.
    3)  Follow the rule that #### in UEFI global variables are upper case hex in VarCheckLib and VarCheckUefiLib.
11. VariablePei: Return error for empty str VariableName to GetVariable.
12. PlatVarCleanupLib:
    1)  Locate VarCheck protocol when using.
    2)  Add lib destructor for unclosed event.
13. FaultTolerantWriteDxe: Mellow DEBUGs about workspace reinit and clean up some "success" messages.
14. PCD:
    1)  Avoid DynamicHii PCD set to override the value at other offsets.
    2)  Fix PcdGetNextToken may get a wrong PCD token.
    3)  Allow SkuId to be changed only once.
    4)  Fix TmpTokenSpaceBufferCount not assigned correctly when DynamicEx PCD is only used in PEI code.
15. Memory profile:
    1)  Check free memory type by CoreUpdateProfile() to improve profile performance in DxeCore.
    2)  Remove unreferenced symbol for memory profile in DxeCore and PiSmmCore.
    3)  Handle "/" character in the PDB path in MemoryProfileInfo application.
16. Use fixed SMM communication buffer.
    1)  Add EDKII_PI_SMM_COMMUNICATION_REGION_TABLE definition in PiSmmCommunicationRegionTable.h.
    2)  Add SmmCommunicationBufferDxe to publish EDKII_PI_SMM_COMMUNICATION_REGION_TABLE.
    3)  Update SMM handler code in PiSmmCore and FirmwarePerformanceSmm to support getting boot record data
        and SMRAM profile data by offset.
    4)  Update FirmwarePerformanceDxe, DxeSmmPerformanceLib, MemoryProfileInfo and VariableInfo applications
        to use fixed SMM communication buffer provided by EDKII_PI_SMM_COMMUNICATION_REGION_TABLE.
    5)  Move CommunicationBuffer from stack to global variable in PiSmmCore.
17. ACPI:
    1)  PiDxeS3BootScriptLib
        1)  Remove a hidden assumption "No SMM driver writes BootScript between SmmReadyToLock
            and S3SleepEntryCallback".
        2)  Add DESTRUCTOR S3BootScriptLibDeinitialize.
        3)  Accept 64-bit LoopTimes.
    2)  Save 64-bit LoopTimes in S3SaveStateDxe and SmmS3SaveState.
    3)  AcpiTableDxe
        1)  Don't uninstall Acpi Sdt Protocol at ReadyToLock.
        2)  Consider version mask when removing tables.
        3)  Not make FADT.{DSDT,X_DSDT} mutual exclusion to have better compatibility as some OS
            has assumption to only consume X_DSDT field even the DSDT address is < 4G.
    3)  Don't allocate below 4 GB in BootGraphicsResourceTableDxe.
18. SmmLockBoxLib:
    1)  Work without EFI_PEI_SMM_COMMUNICATION_PPI in SmmLockBoxPeiLib.
    2)  Add DESTRUCTOR SmmLockBoxSmmDestructor in SmmLockBoxSmmLib.
19. AtaAtapiPassThruDxe
    1)  Select master/slave around DIAG command for IDE mode.
    2)  Return correct status when DRQ is not ready for ATAPI at IDE mode.
    3)  Report early finish of packet read as success at IDE mode.
    4)  Correctly report length of returned data at IDE mode.
    5)  Add DEBUG errors for COMRESET and Port phy not ready at AHCI mode.
    6)  Remove polling on PxCMD.FR flag setting at AHCI mode.
    7)  Update AtaStatusBlock after cmd exec at AHCI mode.
    8)  Ensure GHC.AE bit is always set in Ahci.
20. SCSI:
    1)  ScsiBusDxe
        1)  Fix caller event may never be signaled.
        2)  Only signal caller event when PassThru() succeeds.
        3)  Raise the Tpl of async IO callback to TPL_NOTIFY.
    2)  ScsiDiskDxe
        1)  Recognize EFI_BAD_BUFFER_SIZE.
        2)  Adapt SectorCount when shortening transfers.
        3)  Set block I/O media of SCSI CDROM to read-only.
        4)  Modify FlushBlocksEx() to follow UEFI spec.
        5)  Modify WriteBlocks(Ex)() to follow UEFI spec.
        6)  Close event when SCSI command fails.
        7)  Fix async request retry times info lost issue.
        8)  Add retry scheme for async SCSI I/O command.
        9)  Raise the Tpl of async IO callback to TPL_NOTIFY.
        10) Increase the value of SCSI_DISK_TIMEOUT to 30s.
        11) Fix hang issue when reconnecting an ISCSI device.
        12) Cope with broken "Supported VPD Pages" VPD page.
21. USB:
    1)  Fix GetState() to return absolute value in UsbMouseAbsolutePointerDxe.
    2)  Don't assert when the key read is invalid in UsbKbDxe.
    3)  Fix wrong condition judgment to support usb3.1 dev in UsbBusDxe, UsbBusPei, XhciDxe and XhciPei.
    4)  UsbBusDxe
        1)  Correctly handle the case that USB descriptor length returned is greater than expected.
        2)  Remove redundant host controller reset.
        3)  Reduce the port status polling before port reset.
        4)  Fix system hang when failed to uninstall UsbIo.
    5)  Don't clear port status bits during init in EhciDxe and EhciPei.
    6)  XhciDxe and XhciPei
        1)  Add 1ms delay before access MMIO reg during reset.
        2)  Change short packet debug message to verbose level.
        3)  Add 10ms delay before sending SendAddr cmd to dev.
    7)  XhciDxe
        1)  Fix usb desc length check logic.
        2)  Fix a bug on TRB check in async int transfer.
        3)  Remove TRB when canceling Async Int Transfer.
22. UFS:
    1)  UfsPassThruDxe
        1)  Raise to TPL_NOTIFY when dealing async task.
        2)  Fix to identify 32 bits address mode.
        3)  Wait fDeviceInit be cleared by devices during init.
    2)  UfsPassThruDxe and UfsBlockIoPei
        1)  Ensure the DBC field of UTP PRDT is dword-aligned.
        2)  Fix the bit in UFS_HC_UTRLDBR_OFFSET reg.
        3)  Initialize OCS value to 0x0F.
    3)  Avoid overriding return value in BindingStart of UfsPciHcDxe.
23. PCI:
    1)  PciBusDxe
        1)  Exit pci function loops early if device is not multi-function.
        2)  Fix a hot plug bug.
        3)  Do not dump NULL padding resource descriptor.
        4)  Fix a PCI resource dumping bug.
        5)  Return AddrTranslationOffset in GetBarAttributes.
        6)  Reserve enough bus number for HPC.
        7)  Don't create bogus descriptor if no resources is needed.
        8)  Skip invalid bus number scanning.
        9)  Do not improperly degrade resource.
        10) Look for the right capability in IsSHPC().
        11) Recognize hotplug-capable PCIe ports.
        12) Accept Spec values as BarIndex and Alignment.
    2)  IncompatiblePciDeviceSupportDxe
        1)  Do not use deprecated macros.
        2)  Use MAX_UINTN to match any IDs.
24. NvmExpressDxe:
    1)  Fix wrong logic in GetControllerName().
    2)  Clean up NvmeRead() / NvmeWrite() debug msgs.
    3)  Ensure write-through for NVMe write command.
    4)  Fix bug of handling not null-terminated strings.
    5)  Clean Phase/CqHdbl/SqTdbl fields to restart HC.
    6)  Initialize IoAlign info for an NVMe device.
    7)  Fix invalid queue size when creating IO queues.
    8)  Avoid crashing 'Mode' during OpenProtocol.
    9)  Refine BuildDevicePath, GetNameSpace and GetNextNamespace APIs to follow spec.
    10) Add buffer alignment check in PassThru API.
    11) Add check on the attributes of NVME controller.
    12) Add check for command packet in PassThru.
    13) Add NamespaceId validity check in PassThru.
    14) Fix 'Event' won't be signaled for Admin cmds.
    15) Set the non-blocking I/O feature support bit.
    16) Handling return of write to sq and cq db.
25. PartitionDxe:
    1)  Use proper partition number for MBR.
    2)  Fix some ISO images cannot be recognized properly.
    3)  Add Re-entry handling logic for BindingStop.
26. GraphicsConsoleDxe: Fix resolution out of sync issue.
27. ConSplitterDxe: Rescale ConSplitter Absolute Pointer.
28. ConPlatformDxe: Support GOP created as PCI's grandson.
29. TerminalDxe:
    1)  Avoid checking uninitialized variable in TerminalConInTimerHandler().
    2)  Improve TtyTerm cursor position tracking.
    3)  Optimize TtyTerm cursor motion.
    4)  Handle more keys with TtyTerm.
    5)  Fix driver model bugs that it cannot be started AGAIN using a different terminal type and it doesn't install
        SimpleTextIn/SimpleTextOut when ConIn/ConOut doesn't contain its device path.
30. BdsDxe:
    1)  Do not boot to UI again when BootNext points to UI.
    2)  Skip registering BootManagerMenu if absent.
    3)  Check deferred images before booting to OS.
    4)  Fix bug to run non-first PlatformRecovery####.
    5)  Avoid overwriting PlatformRecovery####.
    6)  Initialize gConnectConInEvent earlier.
31. UefiBootManagerLib
    1)  Do not assume perf entry count has no change.
    2)  Always create MemoryTypeInformation variable.
    3)  Do not pass unnecessary option to boot option.
    4)  Change EfiBootManagerDeleteLoadOptionVariable() to not just remove #### from BootOrder but also remove
        Boot#### variable.
    5)  Make network boot option description more user-friendly.
    6)  Update code to not work on inactive consoles.
    7)  Allocate reserved memory for RAM Disk boot media.
    8)  Fix a boot hang due to Ram Disk boot support.
    9)  Prevent the BmRepairAllControllers routine in an infinite loop.
    10) Skip registering BootManagerMenu if absent.
    11) Re-order the sequences by putting updating memory type information before loading the boot option so that
        the reserved memory usage by HTTP RAM disk boot can be excluded by the memory type information updating.
    12) Rename BootMenuApp to BootManagerMenu.
    13) Fix a bug that may causes S4 fails to resume.
    14) Handle "/" separator in debug path for GCC build.
    15) Enhance short-form expanding logic.
    16) Build meaningful description for Wi-Fi boot option.
32. BootManagerMenuApp:
    1)  Update code to not display itself.
    2)  Fix bug that boots to undesired option.
33. SecurityStubDxe: Defer 3rd party image before EndOfDxe.
34. PerformanceLib:
    1)  Update PerformanceLib instances not to check Identifier as Identifier is for single PERF, not the pair of PERF.
    2)  Update DxeCorePerformanceLib to only support linking with DxeCore.
    3)  Add DEBUG statement when reaching max perf log entries in PeiPerformanceLib.
35. SmbiosManagementDxe:
    1)  Use correct SMBIOS table to print information.
    2)  Use correct VendorGuid and VendorTable to measure.
    3)  Use EFI_D_VERBOSE for internal dump functions.
    4)  Add NominalSpeed in Type 27 to black list.
36. SmbiosDxe: Soften DEBUG messages about table reallocation.
37. EbcDxe: Use EfiBootServicesCode memory for thunks.
38. FvSimpleFileSystemDxe:
    1)  Don't open DevicePath with BY_DRIVER mode.
    2)  Fix assertions when FV is empty.
39. PlatformHookLibSerialPortPpi: Correct module type to PEIM from BASE.
40. Fix undefined behavior in signed left shift.
41. Refine type cast for pointer subtraction.
42. Refine casting expression result to bigger size.
43. Fix undefined behavior in signed left shift.
44. Remove unsupported PcdExpression usage in module INF.
45. Fix Error Level for debug message is not used correctly.
46. NETWORK:
    1)  Update all network drivers to check the received packet size before use it to avoid ASSERT.
    2)  SNP
        1)  Fix a bug in SNP.GetStatus() that the recycled transmitted buffer address may be ignored incorrectly.
        2)  Fix a bug in SNP.Transmit() that EFI_NOT_READY should be returned if UNDI report transmit buffer full.
    3)  MNP: Fix a bug that duplicated records in VLAN saved variable may cause MNP fall into infinite loop.
    4)  iSCSI
        1)  Fix a bug in iSCSI driver binding stop function may return incorrect error status code.
        2)  Fix a bug that "reconnect -r" command may cause iSCSI driver ASSERT.
        3)  Check platform supported ACPI version before publish the iBFT table.
    5)  PXE
        1)  Fix a bug that TTL/ToS parameter is not set correctly in UdpWrite() interface.
        2)  Check the DHCP packet to make sure "BootFileName" is not overloaded before use it in PXE driver.
        3)  Reset DHCP4 child before leaving PXE driver's EFI Load File Protocol, this will allow other programs
            (like HTTP boot) to use DHCP4 service in future.
    6)  IP4
        1)  Stop the timer event before cleaning up the service data to avoid access freed buffer
            in timer callback function.
        2)  Fix a bug which will cause 2nd time PXE boot fail after a system reset during PXE boot.
        3)  Fix a bug that the IP address configuration may not be saved to NV storage in certion condition.
        4)  Fix a bug that Ip4Config2.SetData() may return incorrect status code.
        5)  Fix a bug that invalid DNS address may cause Ip4Config2.SetData() ASSERT.
        6)  Fix a bug that Ip4Config2 couldn't create interface name if there are more than 9 network devices
            in the system.
        7)  Fix bugs that the DestoryChild() function incorrectly cleans up the service before upper layer driver
            is stopped.
        8)  Ip4Config2.SetData() is updated to Ip/Netmask pair before use to avoid invalid combination.
    7)  TCP4
        1)  Fix a bug that ACK is not sent out in certain circumstance.
        2)  Fix a bug which will cause the iSCSI client can not send reset packet correctly during "reconnect -r".
        3)  Addressing TCP Window Retraction problem when window scale factor is used.
    8)  HttpLib
        1)  Fix a bug that HttpLib can't parse Ipv6 address correctly.
        2)  Fix a bug that HttpLib can't parse the last chunked data.
        3)  Fix a bug that parsing port number may fail on X64 platform.
47. HII:
    1)  SetupBrowserDxe
        1)  When failed to submit data and user discard the change, send the discard info to driver
            with EFI_BROWSER_ACTION_CHANGED callback.
        2)  Don't support password which does not have interactive flag. Browser just return EFI_UNSUPPORTED for it.
    2)  HiiDatabaseDxe
        1)  Add error handling code to avoid ASSERT in HiiConfigRoutingRouteConfig when the driver does not
            install the ConfigAccess protocol.
        2)  Returns 'Progress' from HiiConfigRoutingRouteConfig to indicate the failure info if failed
            to save data for EfiVarStore.
    3)  DeviceManagerUiLib: Fix the network device MAC display issue.
    4)  DisplayEngineDxe: Fix some incorrect display issues that exposed by some corner use cases. For example,
        1)  Case one: A form has more than one page, there is no menu in the first page can be highlighted and
            the first menu in the second page can be highlighted. When entering this form, incorrect display appears.
        2)  Case two: Menus in the form occupy the full page, and the length of option string of one menu equals
            to the gOptionBlockWidth specified in the DisplayEngine. At the bottom of current page, user goes
            to another form with a goto menu, when pressing "ESC" to return, incorrect display appears.
    5)  DriverSampleDxe: Fix a bug in generating ConfigAltResp string to provide a correct example that default value
        can be got from ConfigAltResp string.
        
## KNOWN ISSUES
1.  Current SmiHandlerProfile implementation builds profile database at SmmReadyToLock. So the profile for SMI handler
    registered after SmmReadyToLock will be not recorded.
2.  SmmLockBoxDxeLib uses stack for SmmCommunication buffer, RestoreLockBox/RestoreAllLockBoxInPlace
    in SmmLockBoxDxeLib will fail by the SmmCommunication buffer check after SmmReadyToLock.
3.  HII:
    1)  EFI_IFR_IMAGE, EFI_IFR_ANIMATION, EFI_IFR_VARSTORE_DEVICE opcodes are not supported.
    2)  Nested Suppressif/DisableIf/GrayoutIf condition for single statement is not supported.
4.  MNP driver is updated to recycle the TX buffer from SNP asynchronously. The UNDI/SNP implementation must follow
    UEFI 2.6 Appendix E.4.16 to return the recycled transmitted buffer address in UNDI GetStatus command to make it work,
    otherwise an incorrect recycled buffer address will cause a DEBUG version MNP driver ASSERT.
