# [UDK2018]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2018) IntelFrameworkModulePkg Notes

##  NEW FEATURES AND CHANGES
1. Add IoMmu Support in LegacyBiosDxe.
2. Update FwVolDxe to get FV auth status propagated from PEI for PI1.5.
3. Update FwVolDxe to support FFS_ATTRIB_DATA_ALIGNMENT_2 for PI1.5.
4. Update LegacyBiosDxe, KeyboardDxe to use macro to enable/disable page 0.

##  PACKAGE INTERFACE CHANGES
1. Remove the useless Perf codes in BdsDxe and GenericBdsLib.


##  BUG FIXES
1. Fix misuse of AllocateCopyPool that causes heap memory overflow.
2. Update KeyboardDxe and Ps2KeyboardDxe ReadKeyStrokeEx() always return key state.
3. Fix Xcode 9 Beta treating 32-bit left shift as undefined
