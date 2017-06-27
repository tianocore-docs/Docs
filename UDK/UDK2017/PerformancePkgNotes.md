# [UDK2017]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2017) PerformancePkg Notes



##  NEW FEATURES AND CHANGES
1.  Dp_App: Add a new option -c to dump cumulative data.
2.  Dp_App: Support execution break.
3.  Dp_App: Make Dp print help information with -? flag in Shell.
4.  Dp_App: Remove TimerLib dependency, and DxeSmmPerformanceLib can be linked to make DP command generic for both
    normal performance log and SMM performance log.



##  BUG FIXES
1.  Dp_App: Use Image->FilePath to get name for SMM drivers.
2.  Dp_App: Fix the error message "Timer library instance error!"
3.  Dp_App: Replace UnicodeStrToAsciiStr/AsciiStrToUnicodeStr.
4.  Dp_App: Handle "/" separator in debug path for GCC build.


##  KNOWN ISSUES
1.  TscTimerLib instances depends on the Intel ICH chipset ACPI timer to calculate TSC frequency, they may not work on
    the future chipset if the settings for ACPI timer are changed. Comments have been added for this, and AcpiTimerLib
    instances in PcAtChipsetPkg are recommended to be used.

