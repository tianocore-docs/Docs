# [UDK2018]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2018) MdeModulePkg  Notes

1. [New Features and Changes](#new-features-and-changes)
2. [Bug Fixes](#bug-fixes)
3. [Known Issues](#known-issues)


##                                               NEW FEATURES AND CHANGES

1.  PeiCore:
    1)  Handle notification PPI from SEC.
    2)  Install SEC HOB data.
    3)  Support pre memory page allocation.
    4)  Consume new PCD PcdInitValueInTempStack to get the init value in temp stack.

2.  PeiCore and DxeCore:
    1)  Support FFS_ATTRIB_DATA_ALIGNMENT_2.
    2)  Propagate PEI-phase FV authentication status to DXE.
    3)  Support EFI_FIRMWARE_VOLUME_EXT_ENTRY_USED_SIZE_TYPE.

3.  DxeCore:
    1)  Enhance ConvertPages: Incompatible memory types.
    2)  Remove extra connects for UEFI Applications.
4.  PiSmmCore: Install EndOfS3Resume and S3SmmInitDone protocols in registered SMI handlers.

5.  PiSmmIpl:
    1)  Handle CommSize OPTIONAL case in SmmCommunicationCommunicate.
    2)  Remove NX page attribute for SMRAM.

6.  DxeIplPeim: Mark page table as read-only.

7.  Update DxeIplPeim and SectionExtractionPei to remove the hard code alignment adjustment. Section data alignment should be made in the build generation.

8.  Null Pointer Detection: DxeIplPeim and DxeCore are updated to support the feature.
    1)  This feature is used to detect invalid access to NULL pointer and page fault exception will be triggered if such issue is detected. Due to the implementation limitation, accessing any address in page 0 (000-FFF) will be actually detected by this feature. This feature is enabled by BIT0 and BIT1 of        PcdNullPointerDetectionPropertyMask for UEFI and SMM code separately.
    2)  For source legacy drivers which need to access legacy memory at page 0, macro ACCESS_PAGE0_CODE can be used to enclose those statements to avoid unnecessary page fault exception without losing their original       functionalities, if this feature needs to be enabled.
    3)  For binary legacy drivers, legacy OptionROM and legacy compatible OS loader, which will normally access page 0,  BIT7 of PcdNullPointerDetectionPropertyMask can be used, as last resort, to disable this feature right after       EndOfDxe event.
    4)  This feature is disabled by default.
  
9.  Heap Guard: DxeIplPeim, DxeCore and PiSmmCore are updated to support the feature.
    1)  Heap Guard is used to detect heap memory overflow and page fault exception will be triggered if such issue is        detected. It consists of page guard and pool guard, which can be enabled separately or simultaneously by BIT0         and BIT1 of PcdHeapGuardPropertyMask for UEFI memory, and by BIT2 and BIT3 for SMM memory.
    2)  To let Heap Guard get into work, the users still need to enable one or more allocation types of memory       (like EfiBootServicesData), which need to be detected, through PcdHeapGuardPageType and/or PcdHeapGuardPoolType.         These two PCDs control both UEFI and SMM memory.
    3)  For pool memory, BIT7 of PcdHeapGuardPropertyMask is designed to elaborate the detection of buffer overflow from         access growing or declining direction. It's cleared by default (growing direction).
    4)  Due to memory consumption, performance and potential OS boot impact, this feature is not suggested to be enabled         in production BIOS.
    5)  This feature is disabled by default.

10. Stack Guard: DxeIplPeim, DxeCore are updated to support the feature.
    1)  This feature is used to detect the overflow of the whole UEFI stack memory, including the stack of BSP and all         APs. Page fault exception will be triggered if such issue is detected. It can be enabled/disabled by         PcdCpuStackGuard.
    2)  This feature disabled by default.

11. Add IOMMU support.
    1)  Add IOMMU PPI and protocol definitions.
    2)  Update PciBusDxe to support IOMMU. When IOMMU protocol is installed, PciBus calls IOMMU to set access attribute         for the PCI device in Map/Ummap.
    3)  Update PciHostBridgeDxe to support IOMMU. When IOMMU protocol is installed, PciHostBridge just calls IOMMU         AllocateBuffer/FreeBuffer/Map/Unmap.
    4)  Update SdBlockIoPei, EmmcBlockIoPei, UfsBlockIoPei, EhciPei, UhciPei and XhciPei to support IOMMU.
    
12. Add EndOfS3Resume GUID definition, after S3 SMM initialization is done and before S3 boot script is executed, the     GUID will be installed at the end of S3 resume phase as protocol in SMM environment.

13. Add S3SmmInitDone GUID definition, the GUID will be installed as PPI in PEI and protocol in SMM environment.

14. Add SmmMemoryAttribute protocol definition, the protocol is intended for PiSmmCore to be able to change memory page     attributes for the sake of heap guard feature.

15. Add LOCK_BOX_ATTRIBUTE_RESTORE_IN_S3_ONLY lockbox attribute and update LockBoxLib to support it.

16. Add a new API InitializeCpuExceptionHandlersEx in CpuExceptionHandlerLib to support initializing exception handlers     with extra functionalities which need extra init data, such as stack switch for Stack Guard feature.

17. BootScriptExecutorDxe: Remove NX page attribute for reserved FfsBuffer.

18. Update RuntimeDxe and PeiCrc32GuidedSectionExtractLib to consume CalculateCrc32() API in BaseLib.

19. UefiBootManagerLib:
    1)  Support DNS device path description.
    2)  Generate boot description for SD/eMMC.
    3)  Remove the useless perf codes.

20. MemoryTest: Update GenericMemoryTestDxe and NullMemoryTestDxe to handle More Reliable memory type.

21. SmbiosMeasurementDxe: Skip measurement for OEM type and platform code can measure it by self if required.

22. FirmwarePerformancePei: Remove the SEC performance data getting code as SecCore in UefiCpuPkg has added     SecPerformancePpiCallBack to get SEC performance data and build HOB to convey the SEC performance data to DXE phase.

23. PeiDxeDebugLibReportStatusCode: Print partially when format string is too long.

24. SerialDxe:
     1)  Process timeout consistently in SerialRead.
     2)  Handle Timeout change more robustly in SerialSetAttributes.

25. ResetSystem:
    1)  Add PlatformSpecificResetFilter and PlatformSpecificResetHandler PPI and protocol definitions.
    2)  Add PlatformSpecificResetNotification PPI definition.
    3)  Update ResetSystemRuntimeDxe to implement ResetNotification, ResetFilter and ResetHandler protocols.
    4)  Add ResetSystemPei to implement Reset2, ResetNotification, ResetFilter and ResetHandler PPIs.
    5)  Add ResetSystemLib instances, PeiResetSystemLib calls the ResetSystem2() service in the PEI Services Table, and         DxeResetSystemLib calls the ResetSystem() service in the UEFI Runtime Services Table.
    6)  Add ResetUtility library class and BASE instance. The library class that provides services to generate a GUID         specific reset, parse the GUID from a GUID specific reset, and build the ResetData buffer for any type of reset         that requires extra data.
    7)  Update PeiCore to always attempt to use Reset2 PPI first.

26. PCD:
    1)  Update PCD driver to support the optimized PcdDataBase.
    2)  Enable Firmware to retrieve the default setting, add two PCDs PcdSetNvStoreDefaultId and         PcdNvStoreDefaultValueBuffer.
    3)  Remove unused PCD attribute PCD_TYPE_SKU_ENABLED.
    
27. Variable DXE/SMM driver:
    1)  Deprecate EFI_VARIABLE_AUTHENTICATED_WRITE_ACCESS.
    2)  Update GetNextVariableName to follow UEFI 2.7.
    3)  Update code to also support normal format variable storage HOB when the NV variable store is auth format.

28. HII:
     1)  EFI Varstore/Bufffer Varstore BIT field support.

         1)  Add GUID/flags definitions for BitField support.
         2)  Update SetupBrowserDxe, HiiDatabaseDxe, UefiHiiLib and VarCheckHiiLib to handle/check Question which is  stored in the BIT field of EFI Varstore/Bufffer Varstore.
         3)  Add sample cases in DriverSampleDxe to create BIT/UNION varstore and add sample Questions to consume bit/union VarStore.
         
      2)  Update HiiDatabaseDxe to replace the default setting in vfr file with the one set through DynamicHii PCD.
      3)  Update DisplayEngineDxe to add the implementation of HiiPopup protocol which is defined in UEFI2.7 Spec.
      4)  Update DriverSampleDxe to add a sample case to consume HiiPopup protocol.

29. Network:
    1)  Update network stack drivers (DHCP, iSCSI, PXE) to check the "Network Media State" type of AIP protocol, and
        wait the NIC to restore the network connection if EFI_NOT_READY is returned.
    2)  HttpLib: HttpMappingToStatusCode() is updated to handle the new defined HTTP status code
        HTTP_STATUS_308_PERMANENT_REDIRECT in UEFI specification.
    3)  IP4:
    
        1)  Update Ip4Config.SetData() to clean up certain configuration data.
        2)  Ip4 driver is updated to trigger Ip4Config2 protocol to retrieve a default address, if the default IPv4
            address is not available and UseDefaultAddress is set to TRUE.
        3)  Update IP4 stack to support point-to-point link with 31-bit mask (RFC3021).
    4)  NetLib: Add new library interface NetLibDetectMediaWaitTimeout() to support media
        state detection by AIP protocol.

30. Performance Infrastructure: Update EDKII performance infrastructure to log and dump the performance entry as FPDT     record in ACPI FPDT table. The updated performance infrastructure can support to dump performance data in UEFI     Shell and OS both.
    1)  PeiPerformanceLib:

        1)  Update PeiPerformanceLib to convert Perf entry to FPDT record in PEI phase.
        2)  Report FPDT records in PEI phase to DxeCorePerformanceLib through GUID hob.
    2)  SmmCorePerformanceLib:
    
        1)  Update SmmCorePerformanceLib to convert Perf entry to FPDT record in SMM phase.
        2)  Define a new structure smm boot performance table to save the contents and size of FPDT records in SMM
            phase and report the address of smm boot performance table to FirmwarePerformanceDataTableSmm.
    3)  DxeCorePerformanceLib:
    
        1)  Update DxeCorePerformanceLib to convert Perf entry to FPDT record in DXE phase.
        2)  Collect FPDT records in PEI phase through the GUID hob. 
        3)  Collect FPDT records in SMM phase through MM Communication protocol.
        4)  Allocate boot performance table to save all FPDT records and report the address of boot performance table
            to FirmwarePerformanceDataTableDxe.
    4)  FirmwarePerformanceDataTablePei: Add FPDT records into basic boot performance table for S3 phase
    5)  FirmwarePerformanceDataTableSmm: Update FirmwarePerformanceDataTableSmm to receive smm boot performance table address which is reported by SmmCorePerformanceLib.
    6)  FirmwarePerformanceDataTableDxe:
    
        1)  Update FirmwarePerformanceDataTableDxe to receive boot performance table address which is reported by             DxeCorePerformanceLib.
        2)  Update FirmwarePerformanceDataTableDxe to Hook  EFI_SW_DXE_BS_PC_READY_TO_BOOT_EVENT to install ACPI table.
        3)  Remove the macro EXTENSION_RECORD_SIZE, since the extension size can be got through PcdExtFpdtBootRecordPadSize.
        4)  Remove the codes that collect boot records in SMM phase(The codes have been moved to DxeCorePerformanceLib).
        
31. Update ReadKeyStrokeEx() in Ps2KeyboardDxe, TerminalDxe and ConSplitterDxe to always return the key state even     there is no key pressed.

32. AtaAtapiPassThruDxe: Relax PHY detection timeout value from 10ms to 15ms.

33. SdDxe and EmmcDxe: Produce Disk Information Protocol for SD/eMMC devices.

34. UfsPassThruDxe: Produce EFI UFS Device Config Protocol.
35. PciBus:

    1)  Install PciEnumerationComplete protocol after PciIo protocols are installed, instead of after hardware enumeration is completed. The change is to follow the PI spec and also benefits certain implementation that         depends on the PciIo handle in PciEnumerationComplete callback.
    2)  Reserve BUS number for non-root and root hot-plug controllers. Old implementation only reserves for root    hot-plug controllers. 
    
36. Add Translation field to PCI_ROOT_BRIDGE_APERTURE. Translation is used to represent the difference between device    address and host address, if they are not the same on some platforms.



##  BUG FIXES 
1.  PeiCore: Update debug message to print FV handle correctly.
2.  DxeCore:

    1)  Avoid accessing non-owned memory in CoreValidateHandle, CoreDisconnectControllersUsingProtocolInterface() and         CoreOpenProtocol().
    2)  Fix double free pages on LoadImage failure path.
    3)  Fix Interface returned by CoreOpenProtocol.

3.  PiSmmCore:
    1)  Unregister each other for LegacyBoot and ExitBootServices SMI handlers.
    2)  Set ForwardLink to NULL in RemoveOldEntry() to fix potential linked list assertion.
    3)  Fix hang due to already-freed memory dereference.

4.  Fix misuses of AllocateCopyPool in UiApp, BootMaintenanceManagerUiLib, DeviceManageruiLib, UefiHiiLib,     FvSimpleFileSystemDxe and HiiDatabaseDxe.

5.  DxePrintLibPrint2Protocol: Fix error in Precision position calculation.

6.  SmmLockBox: Return updated Length for EFI_BUFFER_TOO_SMALL.

7.  SmmLockBoxDxeLib: Get SmmCommRegion by gEdkiiPiSmmCommunicationRegionTableGuid system configuration table for SMM     communication buffer.
8.  (Smm)S3SaveState: Extract arguments in correct order in the functions for EFI_BOOT_SCRIPT_PCI_CONFIG2_WRITE_OPCODE and EFI_BOOT_SCRIPT_PCI_CONFIG2_READ_WRITE_OPCODE.

9.  PiSmmCoreMemoryAllocationLib: Fix a FreePool() assertion issue in  when PiSmmCore links against     PeiDxeDebugLibReportStatusCode.

10. UefiBootManagerLib:
      1)  Remove unnecessary assertion in BmCharToUnit to avoid system hang.
      2)  Remove assertion when "BootOptionSupport" variable doesn't exist.
      3)  Remove superfluous TimerLib dependency.

11. DxeCapsuleLibFmp:
    1)  Verify nested capsule with FMP. The current logic that uses the ESRT Table does not work because capsules are         processed before the ESRT Table is published at the Ready To Boot event.
    2)  Add more check for the UX capsule.

12. CapsulePei: Sort and merge memory resource entries to handle the case that the memory resource HOBs are reported     differently between BOOT_ON_FLASH_UPDATE boot mode and normal boot mode.

13. FrameBufferBltLib:
      1)  Fix copying of unaligned memory.
      2)  Fix a bug causing display corrupted by using PixelsPerScanLine when calculating the line width.

14. SerialDxe:
      1)  Fix not able to change serial attributes.
      2)  Do not fail reset when SetAttributes is not supported.

15. Variable DXE/SMM driver:
      1)  Delete and lock OS-created MOR variable.
      2)  Delete and lock MOR in the absence of SMM.

16. HII:
     1)  SetupBrowserDxe: Update API IsResetRequired to cache all the reset signals which are triggered by UI         configuration changes. 
     2)  DisplayEngineDxe: Fix a incorrect display issue that exposed by a corner use case.      Use Case: In one page, some new menus may be dynamically inserted between highlight menu and top of         screen menu when some question are refreshed. Incorrect display will appear if the new added menus        cause the highlight menu and previous top of screen menu can't be shown in one page.
     3)  UefiHiiLib: Fix a bug that check the string length incorrectly for String Opcode.
     4)  BootManagerUiLib: Check reset requirement before exiting UiApp, if reset is required, the platform will be reset.
     5)  BootMaintenanceManagerUiLib: Check reset requirement before exiting UiApp, if reset is required, the platform        will be reset.

17. Network:
     1)  Fix a bug in IP4 driver that TxToken is incorrect removed if error happens in transmit.
     2)  Fix a bug in PXE driver that user selected options are ignored in PXE BootMenu from DHCP option 43.
     3)  Fix a bug in IP4 driver that Ip4IpSecProcessPacket() is always called to locate IpSec protocol even the IpSec        protocol has not been installed.
     4)  Fix a bug in DxeNetLib NetbufTrim() function that the NetBuf TotalSize is not checked with 0 before the trim        operation.

18. TerminalDxe: Fix PCANSI mapping for TRIANGLE and ARROW.

19. AtaAtapiPassThru: Unmap DMA buffers after disabling BM DMA.

20. ScsiBusDxe: EFI SCSI I/O Protocol should not be produced on nonexistent LUNs.

21. ScsiDiskDxe: EFI_NO_MEDIA should be returned instead of EFI_MEDIA_CHANGED when media is removed from device.

22. EmmcDxe: Fix a bug that extra data may be erased by EFI_ERASE_BLOCK_PROTOCOL.EraseBlocks().

23. UfsPassThruDxe and UfsBlockIoPei: Set 'DATA SEGMENT LENGTH' field of the UPIU to the number of descriptor bytes to     write.

24. USB:
     1)  EhciDxe: Call EhcFreeUrb to copy the contents of the mapped DMA buffer into the real buffer when sync interrupt         transfer completes.
     2)  XhciPei and XhciDxe: Recover halted endpoint when BABBLE error occurs.
     3)  XhciDxe
    
         1)  Fix a data loss issue in interrupt transfer. The data loss doesn't impact USB keyboard/mouse functionality             but may cause BLE connection random failure.
         2)  Fix DMA buffer map and unmap inconsistency for async interrupt transfer.
     4)  UsbMassStorageDxe: Fix device compatibility issues so that more USB floppies and USB keys can be supported.
    
25. NonDiscoverablePciDeviceDxe: Fix memory override bug in PciIoPciRead interface.

26. NvmExpressDxe:
     1)  Abort the request by resetting the NVMe controller when a timeout occurs for a blocking PassThru request.
     2)  Notify the NVMe controller when system reset happens.
     3)  Fix error status override within NvmExpressPassThru().
     4)  Fix data buffer not mapped for NVMe Write command.

27. SdMmcPciHcDxe: Call SdMmcFreeTrb() to complete a synchronous operation.

28. PciBus: Fix the bug that EfiBusSpecificDriverOverride protocol is not produced for devices containing option rom.

29. PartitionDxe: Fix ProbeMediaStatusEx() to pass the address of a buffer in the stack instead of a NULL pointer to     ReadDisk() interface of EFI_DISK_IO_PROTOCOL.

##  KNOWN ISSUES 
1.  Current SmiHandlerProfile implementation builds profile database at SmmReadyToLock. So the profile for SMI handler  registered after SmmReadyToLock will be not recorded.

2.  HII:
    1)  EFI_IFR_IMAGE, EFI_IFR_ANIMATION, EFI_IFR_VARSTORE_DEVICE opcodes are not supported.
    2)  Nested Suppressif/DisableIf/GrayoutIf condition for single statement is not supported.

3.  MNP driver is updated to recycle the TX buffer from SNP asynchronously. The UNDI/SNP implementation must follow     UEFI 2.6 Appendix E.4.16 to return the recycled transmitted buffer address in UNDI GetStatus command to make it work,     otherwise an incorrect recycled buffer address will cause a DEBUG version MNP driver ASSERT.


