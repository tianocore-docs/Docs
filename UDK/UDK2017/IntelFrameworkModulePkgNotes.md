# [UDK2017]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2017) IntelFrameworkModulePkg Notes

##  NEW FEATURES AND CHANGES
1.  LzmaDecompressLib: Update LZMA to new 16.04 version.
2.  AcpiS3SaveDxe: Consume PcdAcpiS3Enable to control the code.
3.  LegacyBiosDxe: Get SIO data from SIO interface.
4.  Replace UnicodeStrToAsciiStr/AsciiStrToUnicodeStr with
    UnicodeStrToAsciiStrS/AsciiStrToUnicodeStrS.
5.  KeyboardDxe: Execute key notify function at TPL_CALLBACK.
6.  CpuIoDxe: Modify CpuIoDxe to support new IoLib library.
7.  BdsDxe: Use EfiEventEmptyFunction from UefiLib.

##  PACKAGE INTERFACE CHANGES
1.  AcpiS3SaveDxe: Remove S3Ready() functional code.
2.  Remove the following Protocol/PCDs and use the ones defined in MdeModulePkg.
```
      gEfiPs2PolicyProtocolGuid
      PcdPs2KbdExtendedVerification
      PcdPs2MouseExtendedVerification
      PcdFastPS2Detection
```

##  BUG FIXES
1.  LegacyBiosDxe: Fix legacy serial redirection bug.
2.  IdeBusDxe: Fix undefined behavior in signed left shift.
