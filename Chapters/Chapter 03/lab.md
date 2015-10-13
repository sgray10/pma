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



