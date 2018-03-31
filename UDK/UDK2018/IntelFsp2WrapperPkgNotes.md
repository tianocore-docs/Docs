# [UDK2018]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2018) IntelFsp2WrapperPkg Notes
##                            NEW FEATURES AND CHANGES
1. Provide flexibility for platform to pre-allocate FSP UPD buffer.
   2 new PCDs are added
   ```
   gIntelFsp2WrapperTokenSpaceGuid.PcdFspmUpdDataAddress
   gIntelFsp2WrapperTokenSpaceGuid.PcdFspsUpdDataAddress
   ```
