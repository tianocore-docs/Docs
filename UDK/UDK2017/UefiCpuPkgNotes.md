# [UDK2017]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2017) UefiCpuPkg Notes



##  NEW FEATURES AND CHANGES
1.  Update CPU MP drivers to support single CPU configuration.
2.  Add CPU MP Initialize Library and add MP support in CpuDxe.
3.  Add SMM Communication PPI and Handler Modules.
4.  Add SecCore module and Platform Sec Library class/instance.
5.  Add PiSmmCpuDxeSmm and SMM CPU features Library class/instance.
6.  Add STM support.
7.  Add CpuFeatures modules to initialize CPU SDM features.
8.  Add ASSERT to handle local APIC not config properly.

##  PACKAGE INTERFACE CHANGES
1.  Add the following header files under /Include:<br>
      AcpiCpuData.h             - CPU S3 data.<br>
      CpuHotPlug.h              - CPU Hot Plug Data.<br>
2.  Add the following header files under /Include/Guid:<br>
      CpuFeaturesInitDone.h     - CPU Features Init Done PPI/Protocol should be installed after CPU features are initialized.<br>
      CpuFeaturesSetDone.h      - CPU Features Set Done PPI/Protocol should be installed after CPU features configuration are set.<br>
      MicrocodeFmp.h            - gMicrocodeFmpImageTypeIdGuid definition. <br>
      MsegSmram.h               - Defines the HOB GUID used to describe the MSEG memory region allocated in PEI.<br>
3.  Add the following header files under /Include/Library:<br>
      MicrocodeFlashAccessLib.h - Microcode flash device access library.<br>
      MpInitLib.h               - Multiple-Processor initialization Library.<br>
      PlatformSecLib.h          - Defines interface for platform to perform platform specific initialization in SEC phase.<br>
      RegisterCpuFeaturesLib.h  - Register and manage CPU features.<br>
      SmmCpuFeaturesLib.h       - Provides CPU specific functions to support the PiSmmCpuDxeSmm module.<br>
      SmmCpuPlatformHookLib.h   - SMM CPU Platform Hook Library.<br>
4.  Add the following header files under /Include/Protocol:<br>
      SmmCpuService.h           - SMM CPU Service protocol definition.<br>
      SmMonitorInit.h           - STM service protocol definition.<br>
5.  Add the following header files under /Include/Register:<br>
      ArchitecturalMsr.h        - Architectural MSRs definitions.<br>
      Msr/xxxMsr.h              - Non-Architectural MSRs definitions of xxx processors.<br>
      Msr.h                     - All Architectural and non-Architectural MSRs.<br>
      Cpuid.h                   - Include files for CPUID related definitions.<br>
      Microcode.h               - Microcode Definitions.<br>
      SmramSaveStateMap.h       - SMRAM Save State Map Definitions.<br>
      StmApi.h                  - STM API definitions.<br>
      StmResourceDescriptor.h   - STM Resource Descriptor.<br>
      StmStatusCode.h           - STM Status Codes.<br>
6.  Add the following PCDs in UefiCpuPkg.dec file.<br>
      PcdCpuSmmProfileEnable<br>
      PcdCpuSmmProfileRingBuffer<br>
      PcdCpuSmmProfileSize<br>
      PcdCpuSmmBlockStartupThisAp<br>
      PcdCpuSmmStackGuard<br>
      PcdCpuSmmEnableBspElection<br>
      PcdCpuHotPlugSupport<br>
      PcdCpuSmmDebug<br>
      PcdCpuSmmFeatureControlMsrLock<br>
      PcdPeiTemporaryRamStackSize<br>
      PcdCpuSmmStackSize<br>
      PcdCpuSmmCodeAccessCheckEnable<br>
      PcdCpuNumberOfReservedVariableMtrrs<br>
      PcdCpuSmmStmExceptionStackSize<br>
      PcdCpuMsegSize<br>
      PcdCpuFeaturesSupport<br>
      PcdCpuFeaturesUserConfiguration<br>
      PcdCpuFeaturesCapability<br>
      PcdCpuFeaturesSetting<br>
      PcdCpuFeaturesInitAfterSmmRelocation<br>
      PcdCpuFeaturesInitOnS3Resume<br>
      PcdCpuMicrocodePatchAddress<br>
      PcdCpuMicrocodePatchRegionSize<br>
      PcdCpuSmmStaticPageTable<br>
      PcdCpuSmmApSyncTimeout<br>
      PcdCpuSmmSyncMode<br>
      PcdCpuClockModulationDutyCycle<br>
      PcdIsPowerOnReset<br>
      PcdCpuS3DataAddress<br>
      PcdCpuHotPlugDataAddress<br>
7.  MtrrLib: Add API MtrrSetMemoryAttributeInMtrrSettings().<br>
8.  LocalApicLib: Add API GetProcessorLocationByApicId().

##  BUG FIXES
1.  CpuMpPei: Fix pack(1) issue on x64 arch.
2.  CpuMpPei: Fix potential AP mwait wakeup issue.
3.  CpuMpPei: Fix BistData ouput error.

##  KNOWN ISSUES
1.  MtrrSetMemoryAttributeInMtrrSettings() or MtrrSetMemoryAttribute() may
    report Out Of Resource for certain cache settings, while the number of MTRR
    registers is sufficent.


