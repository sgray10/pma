# Chapter 3 Labs

## Lab 3-1

Analyze the malware found in the file Lab03-01.exe using basic dynamic analysis tools.

### Questions
**1. What are this malware’s imports and strings?**

```
Imports found with Dependency Walker:
------------------------------------
[+] KERNEL32.dll
    ExitProcess

NOTE: The lack of imports suggest the executable is packed.


Strings found in Process Monitor:
--------------------------------
jjjjjj
!This program cannot be run in DOS mode.
Rich
.text
`.data
ExitProcess
kernel32.dll
cks=u
ttp=
cks=
CONNECT %s:%i HTTP/1.0
QSRW
PWW
thj@h
PWW
VSWRQ
QVlM
^-m-m<|<|<|M
advapi32
ntdll
user32
Jbh
QQVP
ucj
advpack
StubPath
SOFTWARE\Classes\http\shell\open\commandV
Software\Microsoft\Active Setup\Installed Components\
test
www.practicalmalwareanalysis.com
admin
VideoDriver
WinVMX32-
vmx32to64.exe
SOFTWARE\Microsoft\Windows\CurrentVersion\Run
SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders
PWj
AppData
VQj
ViW
```


**2. What are the malware’s host-based indicators?**

```
Process Monitor caught the following two actions that modified the state of the operating system:

Time               | Process Name | PID  | Operation   | Path                                                           | Result  | Detail
-------------------+--------------+------+-------------+----------------------------------------------------------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------
3:48:26.9261905 PM | Lab03-01.exe | 1316 | CreateFile  | C:\WINDOWS\system32\vmx32to64.exe                              | SUCCESS | Desired Access: Generic Write, Read Attributes, Disposition: Create, Options: Synchronous IO Non-Alert, Non-Directory File, Attributes: N, ShareMode: Write, AllocationSize: 0, OpenResult: Created
3:48:26.9280034 PM | Lab03-01.exe | 1316 | RegSetValue | HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run\VideoDriver | SUCCESS | Type: REG_SZ, Length: 510, Data: C:\WINDOWS\system32\vmx32to64.exe

To confirm the modification to the registry, Regshot found the following registry key had been added:
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run\VideoDriver: 43 3A 5C 57 49 4E 44 4F 57 53 5C 73 79 73 74 65 6D 33 32 5C 76 6D 78 33 32 74 6F 36 34 2E 65 78 65 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
```


**3. Are there any useful network-based signatures for this malware? If so, what are they?**

```
I analyzed the malware with no network functionality. However, the strings highly suggest that 
a call is being made to the following URL:
  
www.practicalmalwareanalysis.com
```

---


## Lab 3-2

Analyze the malware found in the file Lab03-02.dll using basic dynamic analysis tools.

### Questions
**1. How can you get this malware to install itself?**

```
To run a DLL: 
  C:\>rundll32.exe DLLname, EXPORT arguments

The EXPORT value must be a function name or ordinal selected from the exported function table in the DLL
You can use a tool such as PEview or PE Explorer to view the Export table.

When we view the Export table of Lab03-02.dll, we see a install function.
Because of this, we assume we can install by running:
  C:\>rundll32.exe Lab03-02.dll install
```

**2. How would you get this malware to run after installation?**

```
The DLL registered a service:

C:\>sc qc iprip
[SC] GetServiceConfig SUCCESS

SERVICE_NAME: iprip
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\WINDOWS\System32\svchost.exe -k netsvcs
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Intranet Network Awareness (INA+)
        DEPENDENCIES       : RpcSs
        SERVICE_START_NAME : LocalSystem

Simply start the service to run the malware.
```


**3. How can you find the process under which this malware is running?**

```
We can see which svchost.exe has loaded the Lab03-02.dll file.
```


**4. Which filters could you set in order to use procmon to glean information?**

```
We can filter by the PID of the svchost.exe running the malicious DLL.
```


**5. What are the malware’s host-based indicators?**

```
Creation of the following services:
  iprip

Creation of the following registry keys:
  HKLM\SYSTEM\ControlSet001\Enum\Root\LEGACY_IPRIP\0000\Control\*NewlyCreated*: 0x00000000
  HKLM\SYSTEM\ControlSet001\Enum\Root\LEGACY_IPRIP\0000\Service: "IPRIP"
  HKLM\SYSTEM\ControlSet001\Enum\Root\LEGACY_IPRIP\0000\Legacy: 0x00000001
  HKLM\SYSTEM\ControlSet001\Enum\Root\LEGACY_IPRIP\0000\ConfigFlags: 0x00000000
  HKLM\SYSTEM\ControlSet001\Enum\Root\LEGACY_IPRIP\0000\Class: "LegacyDriver"
  HKLM\SYSTEM\ControlSet001\Enum\Root\LEGACY_IPRIP\0000\ClassGUID: "{8ECC055D-047F-11D1-A537-0000F8753ED1}"
  HKLM\SYSTEM\ControlSet001\Enum\Root\LEGACY_IPRIP\0000\DeviceDesc: "Intranet Network Awareness (INA+)"
  HKLM\SYSTEM\ControlSet001\Enum\Root\LEGACY_IPRIP\NextInstance: 0x00000001
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\Enum\0: "Root\LEGACY_IPRIP\0000"
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\Enum\Count: 0x00000001
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\Enum\NextInstance: 0x00000001
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\Security\Security: 01 00 14 80 90 00 00 00 9C 00 00 00 14 00 00 00 30 00 00 00 02 00 1C 00 01 00 00 00 02 80 14 00 FF 01 0F 00 01 01 00 00 00 00 00 01 00 00 00 00 02 00 60 00 04 00 00 00 00 00 14 00 FD 01 02 00 01 01 00 00 00 00 00 05 12 00 00 00 00 00 18 00 FF 01 0F 00 01 02 00 00 00 00 00 05 20 00 00 00 20 02 00 00 00 00 14 00 8D 01 02 00 01 01 00 00 00 00 00 05 0B 00 00 00 00 00 18 00 FD 01 02 00 01 02 00 00 00 00 00 05 20 00 00 00 23 02 00 00 01 01 00 00 00 00 00 05 12 00 00 00 01 01 00 00 00 00 00 05 12 00 00 00
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\Parameters\ServiceDll: "C:\Documents and Settings\rot0xd\Desktop\Labs\Chapter_3L\Lab03-02.dll"
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\Type: 0x00000020
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\Start: 0x00000002
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\ErrorControl: 0x00000001
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\ImagePath: "%SystemRoot%\System32\svchost.exe -k netsvcs"
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\DisplayName: "Intranet Network Awareness (INA+)"
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\ObjectName: "LocalSystem"
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\Description: "Depends INA+, Collects and stores network configuration and location information, and notifies applications when this information changes."
  HKLM\SYSTEM\ControlSet001\Services\IPRIP\DependOnService: 'RpcSs'
  HKLM\SYSTEM\CurrentControlSet\Enum\Root\LEGACY_IPRIP\0000\Control\*NewlyCreated*: 0x00000000
  HKLM\SYSTEM\CurrentControlSet\Enum\Root\LEGACY_IPRIP\0000\Service: "IPRIP"
  HKLM\SYSTEM\CurrentControlSet\Enum\Root\LEGACY_IPRIP\0000\Legacy: 0x00000001
  HKLM\SYSTEM\CurrentControlSet\Enum\Root\LEGACY_IPRIP\0000\ConfigFlags: 0x00000000
  HKLM\SYSTEM\CurrentControlSet\Enum\Root\LEGACY_IPRIP\0000\Class: "LegacyDriver"
  HKLM\SYSTEM\CurrentControlSet\Enum\Root\LEGACY_IPRIP\0000\ClassGUID: "{8ECC055D-047F-11D1-A537-0000F8753ED1}"
  HKLM\SYSTEM\CurrentControlSet\Enum\Root\LEGACY_IPRIP\0000\DeviceDesc: "Intranet Network Awareness (INA+)"
  HKLM\SYSTEM\CurrentControlSet\Enum\Root\LEGACY_IPRIP\NextInstance: 0x00000001
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\Enum\0: "Root\LEGACY_IPRIP\0000"
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\Enum\Count: 0x00000001
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\Enum\NextInstance: 0x00000001
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\Security\Security: 01 00 14 80 90 00 00 00 9C 00 00 00 14 00 00 00 30 00 00 00 02 00 1C 00 01 00 00 00 02 80 14 00 FF 01 0F 00 01 01 00 00 00 00 00 01 00 00 00 00 02 00 60 00 04 00 00 00 00 00 14 00 FD 01 02 00 01 01 00 00 00 00 00 05 12 00 00 00 00 00 18 00 FF 01 0F 00 01 02 00 00 00 00 00 05 20 00 00 00 20 02 00 00 00 00 14 00 8D 01 02 00 01 01 00 00 00 00 00 05 0B 00 00 00 00 00 18 00 FD 01 02 00 01 02 00 00 00 00 00 05 20 00 00 00 23 02 00 00 01 01 00 00 00 00 00 05 12 00 00 00 01 01 00 00 00 00 00 05 12 00 00 00
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\Parameters\ServiceDll: "C:\Documents and Settings\rot0xd\Desktop\Labs\Chapter_3L\Lab03-02.dll"
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\Type: 0x00000020
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\Start: 0x00000002
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\ErrorControl: 0x00000001
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\ImagePath: "%SystemRoot%\System32\svchost.exe -k netsvcs"
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\DisplayName: "Intranet Network Awareness (INA+)"
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\ObjectName: "LocalSystem"
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\Description: "Depends INA+, Collects and stores network configuration and location information, and notifies applications when this information changes."
  HKLM\SYSTEM\CurrentControlSet\Services\IPRIP\DependOnService: 'RpcSs'
```


**6. Are there any useful network-based signatures for this malware?**

```
GET serve.html on host practicalmalwareanalysis.com
```

---


## Lab 3-3

Execute the malware found in the file Lab03-03.exe while monitoring it using basic dynamic analysis tools in a safe environment.

### Questions
**1. What do you notice when monitoring this malware with Process Explorer?**

```
This program creates a process for a malicious svchost.exe
```


**2. Can you identify any live memory modifications?**

```
The svchost.exe is malicious as the version loaded in memory does not match the version stored on disk.
```


**3. What are the malware’s host-based indicators?**

```
A infected svchost running outside the context of winlogon.exe

The presence of the file practicalmalwareanalysis.log indicated the machine is infected.
```


**4. What is the purpose of this program?**

```
Keylogger disguised as svchost.exe
```

---


## Lab 3-4

Analyze the malware found in the file Lab03-04.exe using basic dynamic analysis tools. (This program is analyzed further in the Chapter 9 labs.)

### Questions
***1. What happens when you run this file?***

```
After completion of running, the malware executes the following command:
  "C:\WINDOWS\system32\cmd.exe" /c del C:\DOCUME~1\rot0xd\Labs\CHAPTE~2\Lab03-04.exe >> NUL

We lost our malware sample!
```


***2. What is causing the roadblock in dynamic analysis?***

```
The output of strings on the malware indicates some kind of command line options.
Maybe the malware has to run with the options. 
```


***3. Are there other ways to run this program?***

```
I tried running the malware with the following command line switches:
  -cc
  -c
  -re
  -in

None of these worked.
```
