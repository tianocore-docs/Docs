# [UDK2018]( https://github.com/tianocore/tianocore.github.io/wiki/UDK2018) ShellPkg  Notes

1. [New Features and Changes](#new-features-and-changes)
2. [Bug Fixes](#bug-fixes)
3. [Known Issues](#known-issues)




##                                               NEW FEATURES AND CHANGES

1.  Below changes have been done to improve output readability:
      1) Show all output in a more bright blue/green color.
      2) Change "drivers" to show image name instead of image path in non-SFO mode.
      3) Change "dmpstore" to show name of known variable vendor GUID.
      4) Change "dh":<br>
           1) Show all protocol names in a single line.
           2) Show key information in hilight color instead of blue.
           3) Show protocol instance pointer value when "-v" is supplied.
           4) Support protocols defined in latest UEFI/PI spec.
           5) Show more information for following protocols:
           
            - ImageDevicePath
            - DevicePath
            - LoadedImage
            - BusSpecificDriverOverride
            - BlockIo
            - DebugSupport
            - GraphicsOutput
            - PciIo
            - UsbIo
            - PartitionInfo

2.  Convert "dp" and "tftp" from NULL class library to dynamic command.

3.  Remove unnecessary TimerLib dependency from ShellPkg.dsc.

4.  Change "edit" and "hexedit" to read input through SimpleTextInEx interfaces.

5.  Change "mm" to remove unnecessary IO address limitation (`<=` 0xFFFF).


##                                                       BUG FIXES

1.  Fix "dh" to show correct driver model information.

2.  Fix "ls" to display the file time in local time.

3.  Fix "map" to show correct block IO information when CDROM media status is changed.

5.  Fix "dblk" to use the aligned buffer when calling BlockIo interfaces.

6.  Fix "mkdir" to support creating nested directories.

7.  Fix a bug that fails to change current directory after "map -r".

8.  Fix "ifconfig" to show correct media state.

9.  Fix "edit" hang issue when console max column is bigger than 200.

10. Fix "tftp" hang issue when storage media is full.

11. Fix "hexedit" to be able to access any range of MMIO space.

12. Fix the issue that pressing Ctrl+C before running command unexpecctedly terminates the command.

13. Fix "ping" to not lose the first packet.

##                                                    KNOWN ISSUES

1.  "rm" can delete current working directory via other map name.

2.  "devcfg" does not overlap boot manager functionality.

