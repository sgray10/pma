#Chapter 1: Notes

## Useful Programs
- **PEid**
  - http://www.softpedia.com/get/Programming/Packers-Crypters-Protectors/PEiD-updated.shtml
  - Free static analysis tool used for packer and compiler detection
  - Development and support for PEiD has been discontinued since April 2011.
  - Still the best tool available for packer and compiler detection.

- **PEview**
  - http://wjradburn.com/software/
  - View the PE headers, individual sections, and the import/export tables.
  
- **Dependency Walker**
  - http://www.dependencywalker.com/
  - Lists dynamically linked functions in an executable.

- **Resource Hacker**
  - http://www.angusj.com/resourcehacker/
  - Static analysis utility for viewing, renaming, modifying, adding, deleting, and extracting resources

- **PEBrowse Professional**
  - http://www.smidgeonsoft.prohosting.com/pebrowsepro-file-viewer.html
  - It allows you to look at the bytes from each section and shows the parsed data.
  - Similar to PEview but does a better job of presenting information from the resource (.rsrc) section.

- **PE Explorer**
  - http://www.heaventools.com/
  - Has a rich GUI that allows you to navigate through the various parts of the PE file.
  - It's included resource editor is great for browsing and editing the file’s resources.
  - The tool’s main drawback is that it is not free.
  

## Unicode vs Microsoft 
- Microsoft uses the term wide character string to describe its implementation of Unicode strings
- The Microsoft implementation varies slightly from the Unicode standards.


## Packed and Obfuscated Malware
- Packed and obfuscated code use the following functions to load and gain access to additional functions:
  - LoadLibrary
  - GetProcAddress   


## Portable Executable File Format

| Field            | Information                                                                          |
|------------------|--------------------------------------------------------------------------------------|
| Imports          | Functions from other libraries that are used by the malware                          |
| Exports          | Functions in the malware that are meant to be called by other programs or libraries  |
| Time Date Stamp  | Time when the program was compiled                                                   |
| Sections         | Names of sections in the file and their sizes on disk and in memory                  |
| Subsystem        | Indicates whether the program is a command-line or GUI application                   |
| Resources        | Strings, icons, menus, and other information included in the file                    |

- Data structure that contains information necessary for the Windows OS loader to manage the wrapped executable code

- PE files begin with a header that includes:
  - Information about the code.
  - Type of application.
  - Required library functions.
  - Space requirements.
  
  
## Linked Libraries and Functions
- Code libraries can be connected to the main executable by **linking**.

| Linking Type | Description                                                         |
|--------------|---------------------------------------------------------------------|
| Static       | All code from a library is copied into the executable.              |
| Runtime      | Connects to libraries only when that function is needed.            |
| Dynamic      | OS searches for the necessary libraries when the program is loaded. |

- When Dynamic Linking, Microsoft Windows allow program to import linked functions not listed in PE file header using:
    - LoadLibrary
	- GetProcAddress
	- LdrGetProcAddress	
	- LdrLoadDll


## Common DLLs
| DLL           | Description                                                                                                   |
|---------------|---------------------------------------------------------------------------------------------------------------|
| Kernel32.dll  | Very common DLL. Contains core functionality, such as access and manipulation of memory, files, and hardware. |
| Advapi32.dll  | Provides access to advanced core Windows components such as the Service Manager and Registry.                 |   
| User32.dll    | Contains all the user-interface components for controlling and responding to user actions.                    |
| Gdi32.dll     | Contains functions for displaying and manipulating graphics.                                                  |
| Ntdll.dll     | The interface to the Windows kernel. Executables generally do not import this file directly.                  |
| WSock32.dll   | Networking DLL. Programs that access this most likely connects to a network or performs network-related tasks.|
| Ws2_32.dll    | Same as WSock32.dll                                                                                           |
| Wininet.dll   | Contains higher-level networking functions that implement protocols such as FTP, HTTP, and NTP.               |


## Function Naming Conventions
- Function names with an Ex suffix
  - When Microsoft updates a function and the new function is incompatible with the old one, Microsoft continues to support the old function.
  - The new function is given the same name as the old function, with an added Ex suffix.
  - Functions that have been significantly updated twice have two Ex suffixes in their names.
  - Example: `CreateWindowEx`
  
- Functions ending in A or W
  - Indicates that the function accepts a string parameter and that there are two different versions of the function.
  - A indicates the function accepts ASCII strings.
  - W indicates the function accepts wide strings.
  - Remember to drop the trailing A or W when searching for the function in the Microsoft documentation.
  - Example: `CreateDirectoryW`


## PE File Headers and Sections
| Section       | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| .text         | Contains the executable code                                                 |
| .rdata        | Holds read-only data that is globally accessible within the program          |
| .data         | Stores global data accessed throughout the program                           |
| .idata        | Sometimes present and stores the import function information                 |
| .edata        | Sometimes present and stores the export function information                 |
| .pdata        | Present only in 64-bit executables and stores exception-handling information |
| .rsrc         | Stores resources needed by the executable                                    |
| .reloc        | Contains information for relocation of library files                         |

- **.text**
  - Contains the instructions that the CPU executes.
  - Generally, this is the only section that can execute, and it should be the only section that includes code.

- **.rdata**
  - Contains the import and export information.
  - The same information available from both Dependency and PEView.
  - Can also store other read-only data used by the program.
  - Sometimes a file will contain an .idata and .edata section, which store the import and export information.

- **.data**
  - Contains the program’s global data, which is accessible from anywhere in the program.
  - Local data is not stored in this section, or anywhere else in the PE file.

- **.rsrc**
  - Includes the resources used by the executable that are not considered part of the executable.
  - Examples include icons, images, menus, and strings.
  - Strings can also be stored in the main program, but they are often stored in the .rsrc section for multilanguage support.

