# [UDK2018]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2018) IntelSiliconPkg Notes
##                            NEW FEATURES AND CHANGES
1. Add Intel VTd based IOMMU support for PEI/DXE. They can be used to resist DMA attack on firmware.
   Add Platform VTd Policy protocol and PlatformVTdSampleDxe to demonstrate how to produce the protocol.
   NOTE: The `GetExceptionDeviceList` interface and `PlatformVTdSampleDxe` should only be used for dev/debug purposes,
   and MUST never be used for production builds.
2. Add MicrocodeUpdate capsule support.

