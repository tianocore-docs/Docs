=== UEFI DRIVER WIZARD README ===
Version : 0.11
Date    : 03/03/2012

=== UEFI DRIVBER WIZARD OVERVIEW ===

The UEFI Driver Wizard is designed to aid in the development of 
UEFI Drivers using the EDK II open source project as a development 
environment.  The EDK II provides a cross-platform firmware 
development environment for UEFI.  UEFI Drivers are described in 
the Unified Extensible Firmware Interface Specification, Version 
2.3.1.  There are different categories of UEFI Drivers, and many 
variations of each category.  This wizard provides basic support 
for the most common categories of UEFI drivers.  Many other driver 
designs are possible.  In addition, this wizard provides a 
templates for the various driver-related UEFI Protocols including 
Consoles, Serial Ports, Graphics, Mass Storage, Network Interfaces, 
and User Credentials.

* Information about the UEFI Driver Wizard can be found at:

  http://www.tianocore.org

* Information about the EDK II code base can be found at:

  http://www.tianocore.org
  
* Information about the UEFI Specification can be found at:

  http://www.uefi.org
  
=== UEFI DRIVER WIZARD STATUS ===

Current status: Alpha "as is"

Current capabilities:
* Create new EDK II packages with DEC/DSC files for building UEFI Drivers
* Create new UEFI Driver
  - Generated C code compiles/links using EDK II development environment.
  - Compiled UEFI Driver is loadable with no modifications.
* Create new Protocol template in an existing EDK II package
* Create new GUID template in an existing EDK II package
* Create new Library Class template in an existing EDK II package

=== RUNNING UEFI DRIVER WIZARD ===

Windows Executable:
* Download and install UefiDriverWizard.msi
* Run installed UefiDriverWizard.exe

Python Script under Windows, Linux, UNIX, OS/X:
* Install Python 2.7.2
    http://www.python.org
* Install wxPython 2.8.12.1
    http://www.wxpython.org
* Download UEFI Driver Wizard sources into a new directory
* Run Python script "launch.py"
    
=== GENERATE UefiDriverWizard.msi ===

The following is the set of steps required to generate
UefiDriverWizard.msi from the Python scripts:

* Install Python 2.7.2
    http://www.python.org
* Install wxPython 2.8.12.1
    http://www.wxpython.org
* Install pyInstaller 1.5.1
    http://www.pyinstaller.org
* Install Windows Installer XML 3.5
    http://wix.sourceforge.net
* Download UEFI Driver Wizard sources into a new directory
* Update PYINSTALLER_PATH in GenerateMsi.cmd
* Update WIX_PATH in GenerateMsi.cmd
* Run GenerateMsi.cmd

  Note: Only generation of a 32-bit MSI has been tested.

=== UEFI Driver Wizard Development ===

If you are interested in updating or extending the capabilities of the
UEFI Driver Wizard, then the following tools are required.  

* Install Python 2.7.2
    http://www.python.org
* Install wxPython 2.8.12.1
    http://www.wxpython.org
* Install wxFormBuilder 3.1
    http://wxformbuilder.org

GUI Design Changes
------------------
1) Use wxFormBuilder to open the file UefiDriverWizard.fbp.  
2) Make UI changes using wxFormBuilder.
3) Generate wxPython code and generate code for all the inherited classes.
4) All generated code goes into a subdirectory called InheritedClasses.
5) Merge code changes from InheritedClasses into main directory. 
    Note: wxFormBuilder generates files with TABs.  These must be converted
    to spaces to mach EDK II coding style.
    
UEFI Driver Wizard Template Updates
-----------------------------------
All template files are in the subdirectory called Templates.  Updates to 
these files are used the next time the UEFI Driver Wizard is executed.
Most of the content is plain text that can be easily modified.  Strings 
that are replaced during code generation are placed between << >>.

<<BEGIN>> and <<END>> are special keywords that mark a region of the 
template that can produce 0 or more repeats of the enclosed content.
The use of 0 repeats is valuable for sections of code that are optional
based in the set of features selected.

All of the code generation logic is in the file launch.py. For each 
code generation task, launch.py initializes a dictionary of replacement
strings whose values are set by the GUI forms.  Once the dictionary 
initialization is completed, an internal method applies the dictionary
to files in the Templates directory and writes the result to the proper
destination directory.
