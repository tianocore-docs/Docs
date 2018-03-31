# [UDK2018]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2018) PcAtChipsetPkg Notes
##                            NEW FEATURES AND CHANGES
1. Add `PeiAcpiTimerLib` and change `DxeAcpiTimerLib`.
   `PeiAcpiTimerLib` caches the performance counter frequency in HOB.
   `DxeAcpiTimerLib` uses the cached frequency or re-calculates the frequency
   when it cannot find it in HOB.
##                                  BUG FIXES
1.  Fix a bug in IsaAcpiDxe that may cause keyboard malfunction after
    `"reconnect -r"`.

##                            PACKAGE INTERFACE CHANGES
1.  Add the following PCDs in PcAtChipsetPkg.dec file.
```
      PcdInitialValueRtcRegisterA
      PcdInitialValueRtcRegisterB
      PcdInitialValueRtcRegisterD
```
