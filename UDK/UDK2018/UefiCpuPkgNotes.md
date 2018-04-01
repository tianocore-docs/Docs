# [UDK2018]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2018) UefiCpuPkg  Notes

1. [New Features and Changes](#new-features-and-changes)
2. [Package Interface Changes](#package-interface-changes)
3. [Bug Fixes](#bug-fixes)


##                                               NEW FEATURES AND CHANGES
1.  BaseXApicLib & BaseXApicX2ApicLib: Support AMD.

2.  CpuCommonFeaturesLib:
     1) Enable PPIN (Protected Processor Inventory Number) feature.
     2) Remove SENTER feature because it's merged to SMX feature.
     3) Enable LMCE (Local Machine Check Exception) feature.
     4) Enable PROC_TRACE (Processor Trace) feature.

3.  PiSmmCommunicationSmm: Deprecate SMM Communication ACPI Table.

4.  CpuExceptionHandlerLib: Use separate stack for exception handlers when stack guard is enabled.

5.  PiSmmCpuDxeSmm:
     1) Combine 2 separate INIT-SIPI-SIPI in S3 resume path to improve S3 boot performance.
     2) Produce EDKII_SMM_MEMORY_ATTRIBUTE_PROTOCOL. It's used by SMM heap gurad feature.
     3) Enable NULL pointer detection in SMM environment.

6.  S3Resume2Pei:
     1) Signal EdkiiEndOfS3Resume event in the end of S3 resume path.
     2) Signal EdkiiS3SmmInitDone event after S3 SMM environment is initialized and before boot script is executed.

##                                             PACKAGE INTERFACE CHANGES
1.  Remove depecated macros in Include/Library/MtrrLib.h:
```

      FIRMWARE_VARIABLE_MTRR_NUMBER
      MTRR_LIB_IA32_MTRR_CAP
      MTRR_LIB_IA32_MTRR_CAP_VCNT_MASK
      MTRR_LIB_IA32_MTRR_FIX64K_00000
      MTRR_LIB_IA32_MTRR_FIX16K_80000
      MTRR_LIB_IA32_MTRR_FIX16K_A0000
      MTRR_LIB_IA32_MTRR_FIX4K_C0000
      MTRR_LIB_IA32_MTRR_FIX4K_C8000
      MTRR_LIB_IA32_MTRR_FIX4K_D0000
      MTRR_LIB_IA32_MTRR_FIX4K_D8000
      MTRR_LIB_IA32_MTRR_FIX4K_E0000
      MTRR_LIB_IA32_MTRR_FIX4K_E8000
      MTRR_LIB_IA32_MTRR_FIX4K_F0000
      MTRR_LIB_IA32_MTRR_FIX4K_F8000
      MTRR_LIB_IA32_VARIABLE_MTRR_BASE
      MTRR_LIB_IA32_VARIABLE_MTRR_END
      MTRR_LIB_IA32_MTRR_DEF_TYPE
      MTRR_LIB_MSR_VALID_MASK
      MTRR_LIB_CACHE_VALID_ADDRESS
      MTRR_LIB_CACHE_MTRR_ENABLED
      MTRR_LIB_CACHE_FIXED_MTRR_ENABLED
```

2.  MtrrLib: Add API MtrrSetMemoryAttributesInMtrrSettings().

3.  Add CPUID definitions for AMD: Include/Register/Amd/Cpuid.h.


##                                                       BUG FIXES

1.  CpuCommonFeaturesLib: Fix a bug that wrongly enables SMX/VMX.

2.  CpuMpDxe: Fix a bug that may cause S4 resume failure.

3.  MtrrLib: Re-implement the library to guarantee optimal MTRR settings can always be calculated.

4.  MpInitLib:
     1) Completely initialize AP when AP is re-enabled.
     2) Fix a bug that causes AP enters timer interrupt handler in the source level debug enabled environment.
     3) Fix a bug that timer interrupt is disabled in new BSP after SwitchBSP().
     4) Fix a bug that causes system hang due to below-1MB free memory cannot be found in PEI phase for AP wake up         buffer.
     5) Fix a bug that may cause AP cannot be waken up due to wrong references of StartupApSignal.

5.  CpuExceptionHandlerLib: Fix a bug that causes exception information cannot be dumped in release build.

6.  PiSmmMCpuDxeSmm: Fix potential infinite loop issue when SMM profile is enabled.

7.  SecCore: Fix a bug to guarantee correct SEC performance data can be got through SecPerformancePpi.

8.  CpuCommonFeaturesLib:
     1) Fix AESNI feature detection.
     2) Fix Haswell CPU hang with 50% throttling.
     3) Do not unconditionally enable MCA even it's disabled.

