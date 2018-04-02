# [UDK2018]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2018) SecurityPkg  Notes

1. [New Features and Changes](#new-features-and-changes)
2. [Package Interface Changes](#package-interface-changes)
3. [Bug Fixes](#bug-fixes)


##                                               NEW FEATURES AND CHANGES
1. **Authenticated variable and secure boot**
    1) Update `SecureBootConfigDxe` to present DBX content in 2-layer format
    2) Change Trusted cert policy from whole signer's certificate stack to top-level       issuer cert `tbscertificate + SignerCert CN` for better management compatibility.       Hash is used to reduce storage overhead.

2. **TCG**
    1) Perform TPM2.0 orderly shutdown to incorporate with platform reset.
    2) Define new Pre-Hashed FV PPI to avoid duplicated hash calucation during measure.
    3) Enable TPM2.0 interrupt support. 2 pcds are exposed to report reconfigurable and       non-reconfigurable TPM interrupt resources
    4) Support command cancel if TPM2.0 command execution timeouts
    5) `OpalPasswordPei` is added to fix DMA operation abort issue in S3 path caused by       IOMMU feature.
    6) Apply more checks in TPM1.2, TPM2.0 command lib to fix memory corruption vulnerability

3. **Misc**
    1) Remove `RngTest` application.
    2) Implement `VerifySignature` interface in `Pkcs7Verify` Protocol.


##                           PACKAGE INTERFACE CHANGES
1. Counter based auth variable is not supported anymore. SetVariable with attribute    `EFI_VARIABLE_AUTHENTICATED_WRITE_ACCESS` will return `EFI_INVALID_PARAMETER`. Deletion    of existing counter based auth variable is still allowed when user physical presence    is asserted.

2. `PcdTpm2CurrentIrqNum` and `PcdTpm2PossibleIrqNumBuf` are introduced to report TPM interrupt    resource in a platform. `PcdTpm2CurrentIrqNum` is reported in `_CRS` while `PcdTpm2PossibleIrqNumBuf`    is reported by `_PRS`.

3. All `TrEE` libraries and drivers are removed. A platform should use `Tcg2` libraries and drivers as described below.
<pre>
   Include/Guid/TrEEConfigHii.h              < - Include/Guid/Tcg2ConfigHii.h
   Include/Guid/TrEEPhysicalPresenceData.h   < - Include/Guid/Tcg2PhysicalPresenceData.h
   Include/Library/TrEEPhysicalPresenceLib.h < - Include/Library/Tcg2PhysicalPresenceLib.h
   Include/Library/TrEEPpVendorLib.h         < - Include/Library/Tcg2PpVendorLib.h
   Library/TrEEPpVendorLibNull               < - Library/Tcg2PpVendorLibNull
   Library/DxeTrEEPhysicalPresenceLib        < - Library/DxeTcg2PhysicalPresenceLib
   Library/Tpm2DeviceLibTrEE                 < - Library/Tpm2DeviceLibTcg2
   Tcg/TrEEConfig                            < - Tcg/Tcg2Config
   Tcg/TrEEPei                               < - Tcg/Tcg2Pei
   Tcg/TrEEDxe                               < - Tcg/Tcg2Dxe
   Tcg/TrEESmm                               < - Tcg/Tcg2Smm
</pre>

##                            BUG FIXES
1. Return `NOT_IMPLEMENTED` for Non Vendor specific PPs that are not supported in firmware.

2. Reject illegal `PCR` bank allocation physical presence request

3. Fix `HashInterfaceHob` overflow issue




