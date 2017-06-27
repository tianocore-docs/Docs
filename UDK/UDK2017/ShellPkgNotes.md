# [UDK2017]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2017) ShellPkg Notes



##  NEW FEATURES AND CHANGES
1.  Support features introduced in Shell Specification 2.2:
    1)  Support "-nc" flag in "disconnect"
    2)  Support special output file "NULL"
    3)  Support to change background and foreground colors in "cls"
    4)  Support "-fwui" flag in "reset"
    5)  Support "-sfo" flag in "dmpstore"
    6)  Support "decode" flag to dump GUID/GuidName mappings in "dh"
    7)  Support "-ec" flag in "pci"
    8)  Support "-n" and "-s" flags in "comp"
    9)  Support "mod", "modf", "modp" and "modh" in "bcfg"
    10) Support "-exit" in Shell core
    11) Support new data formats "=0x", "=0X", "=H", "=h", "=S", "=L" and "=P" in "setvar"

2.  Enhance "dp" to be platform independent
3.  Add Persistent Memory support in "memmap"
4.  Support to decode entries defined in SMBIOS Spec 3.1.0 and 3.1.1 in "smbiosview"
5.  Add gUefiShellFileGuid definition referring to the FILE_GUID in ShellPkg/Application/Shell/Shell.inf
6.  Always dump the extended configuration space for PCIE device in "pci"
7.  Dump memory map information for all memory types in "memmap"
8.  Remove unnecessary ShellPkg/Readme.txt


##  BUG FIXES
1.  Fix "echo" to handle special characters properly
2.  Fix "hexedit" to avoid hang when saving file
3.  Fix "cd" to change to root directory when using "cd \"
4.  Fix "mv" to deny moving parent of current directory in "mv"
5.  Fix ">v" and ">>v" to update environment variable properly
6.  Add missing header lines for Standard Format Output of "cls"
7.  Fix "cd" to support "fsx:dir"
8.  Fix Shell core to invoke shell command properly when command parameters contain spaces
9.  Fix "smbiosview" to display properly for Type 0/3/10 entries
10. Fix Shell core not able to run "startup.nsh"
11. Fix "devtree" to avoid memory leak
12. Fix "alias" to support upper-case alias
13. Fix "parse" to handle Unicode stream from pipe correctly


##  KNOWN ISSUES
1.  "rm" can delete current working directory via other map name.
2.  "devcfg" does not overlap boot manager functionality.


