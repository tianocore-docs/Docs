# [UDK2017]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2017) SourceLevelDebugPkg Notes



##  NEW FEATURES AND CHANGES
1. Use PeCoffSerachImageBase() from MdePkg and remove the local implementation.
2. Use APIC Base MSR from ArchitecturalMsr.h.
3. Update DebugAgent to make sure the Local APIC SoftwareEnable bit is set before
   using the Local APIC Timer.
4. Add nasm source files.


##  BUG FIXES
1. Avoid to re-init IDT table again at SMI entry.
2. Fix mMailboxPointer is used before set issue.


