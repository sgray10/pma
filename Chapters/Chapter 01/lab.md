# Chapter 1 Labs

## Lab 1-1This lab uses the files Lab01-01.exe and Lab01-01.dll. Use the tools and techniquesdescribed in the chapter to gain information about the files andanswer the questions below.### Questions**1. Upload the files to http://www.VirusTotal.com/ and view the reports. Does either file match any existing antivirus signatures?**

```
- Lab01-01.exe                                            - Lab01-01.dll
  - ALYac - Trojan.Agent.16384SS                            - ALYac - Trojan.Agent.Waski
  - AVG - Agent5.CDE                                        - AVG - Pakes2_c.LCM
  - AVware - Trojan.Win32.Generic!BT                        - AVware - Trojan.Win32.Generic!BT
  - Agnitum - Trojan.Agent!ibNK9H/HlPg                      - Avast - Win32:Malware-gen
  - AhnLab-V3 - Trojan/Win32.Agent                          - Avira - TR/Dldr.Waski.163840.1
  - Antiy-AVL - Trojan/Win32.TSGeneric                      - Cyren - W32/Trojan.PXBS-7022
  - Avast - Win32:Malware-gen                               - ESET-NOD32 - a variant of Generik.TGEWDD
  - Avira - TR/Rogue.11196274                               - Ikarus - Trojan.SuspectCRC
  - Cyren - W32/Trojan.CZAN-7287                            - NANO-Antivirus - Trojan.Win32.Waski.dtkvsp
  - ESET-NOD32 - a variant of Win32/Agent.WOM               - Rising - PE:Malware.RDM.11!5.11[F1]
  - GData - Win32.Trojan.Agent.RE19WZ                       - Symantec - Trojan.Gen.2
  - Ikarus - Trojan.Rogue                                   - TheHacker - Trojan/Generik.TGEWDD
  - NANO-Antivirus - Trojan.Win32.Rogue.davsrf              - TrendMicro - TROJ_GEN.R08OC0EHD15
  - Qihoo-360 - Win32/Trojan.8b5                            - VIPRE - Trojan.Win32.Generic!BT
  - Symantec - Trojan.Gen.2
  - TheHacker - Trojan/Agent.wom
  - TrendMicro - TROJ_GEN.R08JC0ODD15
  - VIPRE - Trojan.Win32.Generic!BT
```

**2. When were these files compiled?**```Lab01-01.exe: Compilation timestamp 2010-12-19 16:16:19 
Lab01-01.dll: Compilation timestamp 2010-12-19 16:16:38
```

**3. Are there any indications that either of these files is packed or obfuscated? If so, what are these indicators?**

```
Lab01-01.exe
------------
Name    Virtual address    Virtual size    Raw size   Entropy     MD5 
.text    4096              2416            4096       4.45        7e39ebe7cdeda4c636d513a0fe140ff4 
.rdata   8192              690             4096       1.13        2de0f3a50219cb3d0dc891c4fbf6f02a 
.data    12288             252             4096       0.44        f5e2ba1465f131f57b0629e96bbe107e 

The PE sections of the file do not immediately suggest that a packer was used.

VirusTotal Packers identified:
  1) Armadillo v1.71 

--------------------------------------------------------------------------------------------------

Lab01-01.dll
------------
Name    Virtual address    Virtual size    Raw size   Entropy     MD5 
.text   4096               926             4096       1.90        65d3ddf9778db8d01e57b5825fbd93ad 
.rdata  8192               147398          147456     0.03        530532a38a38ea1219e691b8f16d10e9 
.data   155648             108             4096       0.11        0211086333be22ae2620b568fde46fe3 
.reloc  159744             516             4096       0.26        a082f3572d17cd40272b3bcfd96b7b2d 

The PE sections of the file do not immediately suggest that a packer was used.

VirusTotal Packers identified:
  1) Armadillo v1.xx - v2.xx 
```

**4. Do any imports hint at what this malware does? If so, which imports are they?**
```
Lab01-01.exe 
------------
[+] KERNEL32.dll
    MapViewOfFile        --,
    UnmapViewOfFile        |
    FindFirstFileA         |
    FindNextFileA          |
    FindClose              +---> These functions indicate access to the filesystem.
    CopyFileA              |
    CloseHandle            |
    CreateFileMappingA   --'
    CreateFileA
    IsBadReadPtr
                       
[+] MSVCRT.dll
    _except_handler3
    __p__fmode
    malloc
    _adjust_fdiv
    __setusermatherr
    __p__commode
    __p___initenv
    _controlfp
    exit
    _XcptFilter
    __getmainargs
    _exit
    _stricmp
    _initterm
    __set_app_type

--------------------------------------------------------------------------------------------------

Lab01-01.dll 
------------
[+] KERNEL32.dll
    OpenMutexA
    CreateMutexA
    Sleep
    CloseHandle
    CreateProcessA    ---> This indicates the malware will spawn additional processes.

[+] WS2_32.dll        ---> The use of this .dll indicates the malware will talk over the network
    socket
    closesocket
    inet_addr
    send
    WSACleanup
    WSAStartup
    connect
    shutdown
    htons
    recv

[+] MSVCRT.dll
    strncmp
    _initterm
    _adjust_fdiv
    malloc
    free
```
**5. Are there any other files or host-based indicators that you could look for on infected systems?**

```
The exe's .data section contains a reference to "kerne132.dll"
I am assuming this is an attempt to disguise their dll as "kernel32.dll"

The presence of "kerne132.dll" could indicate a system's compromise.
```
**6. What network-based indicators could be used to find this malware on infected machines?**

```
The dll's .data section contains a reference to "127.26.152.13"

Any network traffic being sent to or received from this ip address should be investigated, if not blocked. 
```
**7. What would you guess is the purpose of these files?**
```
The malware seems to copy files and uses network functionality.
I would guess that the malware copies targeted files and sends them to a remote server.
Maybe spawnning additional processes for persistence.```

---
## Lab 1-2Analyze the file Lab01-02.exe.
###Questions**1. Upload the Lab01-02.exe file to http://www.VirusTotal.com/. Does it match any existing antivirus definitions?**```
Antivirus               Result                                        Update  
------------------------------------------------------------------------------
ALYac                   Trojan.Startpage.3072                         20151004  
AVG                     Downloader.Generic12.CMEY                     20151003  
AVware                  Trojan.Win32.Generic!BT                       20151003  
Agnitum                 Trojan.CL.Agent!SYJ1YyE/ZV4                   20151003  
AhnLab-V3               Trojan/Win32.StartPage                        20151003  
Antiy-AVL               Trojan/Win32.SGeneric                         20151004  
Avast                   Win32:Malware-gen                             20151004  
CAT-QuickHeal           Trojan.Dyname.r3                              20151003  
ClamAV                  Win.Trojan.Agent-328471                       20151002  
Comodo                  UnclassifiedMalware                           20151003  
Cyren                   W32/Trojan.UCOC-9169                          20151004  
DrWeb                   Trojan.Click3.12740                           20151004  
ESET-NOD32              Win32/TrojanClicker.Agent.NVM                 20151003  
Fortinet                W32/TrojanClicker_Agent.NVM!tr                20151004  
GData                   Win32.Trojan.Agent.JV4OJM                     20151003  
Ikarus                  Win32.Malware                                 20151003  
Kingsoft                Win32.Malware.Heur_Generic.A.(kcloud)         20151004  
McAfee                  RDN/Generic Downloader.x!lt                   20151004  
McAfee-GW-Edition       RDN/Generic Downloader.x!lt                   20151003  
Microsoft               Trojan:Win32/Dynamer!ac                       20151003  
NANO-Antivirus          Trojan.Win32.RP.cwxtpf                        20151004  
Qihoo-360               HEUR/Malware.QVM11.Gen                        20151004  
Sophos                  Mal/Generic-S                                 20151003  
Symantec                Trojan.Gen.2                                  20151003  
Tencent                 Win32.Trojan.Downloader.Dyzr                  20151004  
TheHacker               Posible_Worm32                                20151002  
TrendMicro              TROJ_GEN.R08JC0EFO15                          20151004  
VIPRE                   Trojan.Win32.Generic!BT                       20151004  
ViRobot                 Trojan.Win32.S.StartPage.3072[h]              20151003  
Zillya                  Trojan.Agent.Win32.549706                     20151003  
nProtect                Trojan/W32.Agent.3072.PM                      20151002  
```**2. Are there any indications that this file is packed or obfuscated? If so, what are these indicators? If the file is packed, unpack it if possible.**```
Name    Virtual address    Virtual size    Raw size   Entropy     MD5 
UPX0    4096               16384           0          0.00        d41d8cd98f00b204e9800998ecf8427e 
UPX1    20480              4096            1536       7.07        ad0f236c2b34f1031486c8cc4803a908 
UPX2    24576              4096            512        2.80        f998d25f473e69cc89bf43af3102beea 

Looking at the PE sections, we can see the following indicators:
  1) The names of the sections are not the normal .text, .data, etc
  2) The virtual size of the sections in memory are much larger than the raw size stored on disk.

These clues indicate that a packer was used in this executable.

VirusTotal Packers identified:
  1) UPX 
```**3. Do any imports hint at this program’s functionality? If so, which imports are they and what do they tell you?**```
Packed:
-------
[+] ADVAPI32.dll
    CreateServiceA ---> Indicates the malware creates a service, possibly for persistence.

[+] KERNEL32.DLL
    VirtualFree
    ExitProcess
    VirtualProtect
    LoadLibraryA   ----> Indicates that the malware will load .dll not listed in the PE headers.
    VirtualAlloc
    GetProcAddress

[+] MSVCRT.dll
    exit

[+] WININET.dll    ---> The use of this .dll indicates the malware will talk over the network
    InternetOpenA
    
--------------------------------------------------------------------------------------------------
    
Unpacked:
---------    
[+] ADVAPI32.dll
    CreateServiceA ----------------,              
    StartServiceCtrlDispatcherA  --'--> Indicates the malware creates a service, probably for persistence.
    OpenSCManagerA

[+] KERNEL32.DLL
    SystemTimeToFileTime
    GetModuleFileNameA
    CreateWaitableTimerA
    ExitProcess
    OpenMutexA
    SetWaitableTimer
    WaitForSingleObject
    CreateMutexA
    CreateThread

[+] MSVCRT.dll
    _exit
    _XcptFilter
    exit
    __p___initenv
    __getmainargs
    _initterm
    __setusermatherr
    _adjust_fdiv
    __p__commode
    __p__fmode
    __set_app_type
    _except_handler3
    _controlfp

[+] WININET.dll    ---> The use of this .dll indicates the malware will talk over the network
    InternetOpenA
    InternetOpenUrlA

```**4. What host or network-based indicators could be used to identify this malware on infected machines?**
```
We see references to the following strings in the unpacked executable's .data:
    1) "MalService" - Could be flagged as a malicious service for a host-based indicator.
    2) "http://www.malwareanalysisbook.com" - Malicious URL for a network-based indicator
```
## Lab 1-3Analyze the file Lab01-03.exe.### Questions**1. Upload the Lab01-03.exe file to http://www.VirusTotal.com/. Does it match any existing antivirus definitions?**```
Antivirus               Result                                        Update
------------------------------------------------------------------------------
ALYac                   Packer.FSG.A                                  20151004
AVG                     Generic4_c.APWM                               20151003
AVware                  Trojan.Win32.Generic!BT                       20151003
Ad-Aware                Packer.FSG.A                                  20151004
Agnitum                 Trojan.Genome!qjszR3auxbA                     20151003
Antiy-AVL               Trojan/Win32.SGeneric                         20151004
Arcabit                 Packer.FSG.A                                  20151003
Avast                   Win32:Malware-gen                             20151004
Avira                   TR/Emoneg.4752                                20151003
BitDefender             Packer.FSG.A                                  20151003
CAT-QuickHeal           Trojan.gen.r3                                 20151003
Comodo                  TrojWare.Win32.Trojan.Inor.B_10               20151003
Cyren                   W32/SuspPack.DH.gen!Eldorado                  20151004
DrWeb                   Trojan.Click2.16518                           20151004
ESET-NOD32              Win32/TrojanClicker.Agent.NVN                 20151003
Emsisoft                Packer.FSG.A (B)                              20151004
F-Prot                  W32/SuspPack.DH.gen!Eldorado                  20150929
F-Secure                Packer.FSG.A                                  20151003
Fortinet                W32/Malware_fam.NB                            20151004
GData                   Packer.FSG.A                                  20151003
Ikarus                  Trojan.Win32.Genome                           20151003
Jiangmin                Trojan/Genome.bfoy                            20151003
K7AntiVirus             Spyware ( 004ce65c1 )                         20151003
K7GW                    Spyware ( 004ce65c1 )                         20151003
Kaspersky               UDS:DangerousObject.Multi.Generic             20151004
Kingsoft                Win32.Troj.Genome.(kcloud)                    20151004
Malwarebytes            Trojan.Agent.MWL                              20151003
McAfee                  RDN/Generic.dx!dfj                            20151004
McAfee-GW-Edition       BehavesLike.Win32.Downloader.xz               20151003
MicroWorld-eScan        Packer.FSG.A                                  20151003
NANO-Antivirus          Trojan.Win32.Inor.getjo                       20151004
Qihoo-360               Malware.Radar01.Gen                           20151004
Rising                  PE:Trojan.Proxy.Win32.Small.gs!1309360[F1]    20151003
Sophos                  Mal/Packer                                    20151003
Symantec                Trojan.ADH                                    20151003
Tencent                 Win32.Trojan.Win32.Huzk                       20151004
TheHacker               Trojan/Genome.ssrc                            20151002
TrendMicro              TROJ_SPNR.30E214                              20151004
TrendMicro-HouseCall    TROJ_SPNR.30E214                              20151004
VBA32                   Trojan.Genome.ss                              20151003
VIPRE                   Trojan.Win32.Generic!BT                       20151004
ViRobot                 Trojan.Win32.A.Genome.4752[h]                 20151003
Zillya                  Trojan.Genome.Win32.112441                    20151003
nProtect                Trojan/W32.Small.4752.C                       20151002
```**2. Are there any indications that this file is packed or obfuscated? If so, what are these indicators? If the file is packed, unpack it if possible.**```
Name    Virtual address    Virtual size    Raw size   Entropy     MD5 
t       4096               12288           0          0.00        d41d8cd98f00b204e9800998ecf8427e       
ta      16384              4096            652        7.36        dcbb3117347a183b93cc9e50e09abd92       
a       20480              4096            512        4.51        83d2bc9613dfc4bc5c714214023f386f 

Looking at the PE sections, we can see the following indicators:
  1) The names of the sections are not the normal .text, .data, etc
  2) The virtual size of the sections in memory are much larger than the raw size stored on disk.

These clues indicate that a packer was used in this executable.

VirusTotal Packers identified:
  1) FSG 
  2) FSG 
  3) FSG v1.00 (Eng) -> dulek/xt 

```**3. Do any imports hint at this program’s functionality? If so, which imports are they and what do they tell you?**```
[+] KERNEL32.dll -----,
    LoadLibraryA      +-> The lack of .dll in the PE headers paired with the LoadLibrary call strongly indicate
    GetProcAddress ---'   that that the malware will dynamically load it's functionality at runtime. Unfortunately, 
                          this could be anything and we can't know without unpacking.
```**4. What host- or network-based indicators could be used to identify this malware on infected machines?**```Packer identified as "FSG 1.0 -> dulek/xt"
Attempts to unpack this one prove much more difficult than UPX.
Unfortunately, without unpacking this executable, I cannot determine any indicators.
```## Lab 1-4
Analyze the file Lab01-04.exe.### Questions**1. Upload the Lab01-04.exe file to http://www.VirusTotal.com/. Does it match any existing antivirus definitions?**```
Antivirus                   Result                                            Update  
--------------------------------------------------------------------------------------
Avast                       Win32:Evo-gen [Susp]                              20151004  
Qihoo-360                   Win32/Trojan.67a                                  20151004  
Tencent                     Win32.Trojan-downloader.Generic.Wmja              20151004  
Kingsoft                    Win32.Troj.Undef.(kcloud)                         20151004  
Cyren                       W32/Heuristic-217!Eldorado                        20151004  
F-Prot                      W32/Heuristic-217!Eldorado                        20150929  
Fortinet                    W32/Dloader.AC                                    20151004  
ByteHero                    Virus.Win32.Part.a                                20151004  
Microsoft                   TrojanDownloader:Win32/Small                      20151003  
CAT-QuickHeal               TrojanDownloader.Small.r3                         20151003  
Antiy-AVL                   Trojan[Downloader:HEUR]/Win32.Unknown             20151004  
Jiangmin                    Trojan/Invader.cxf                                20151003  
TheHacker                   Trojan/Downloader.small                           20151002  
NANO-Antivirus              Trojan.Win32.Kazy.cwxmfl                          20151004  
AVware                      Trojan.Win32.Generic!BT                           20151003  
VIPRE                       Trojan.Win32.Generic!BT                           20151004  
Arcabit                     Trojan.Heur.RP.E9A4ED                             20151003  
DrWeb                       Trojan.DownLoader5.60705                          20151004  
Agnitum                     Trojan.DL.Small!io4/0V8aERQ                       20151003  
ClamAV                      Trojan.Agent-303702                               20151002  
K7AntiVirus                 Trojan-Downloader ( 000074d71 )                   20151003  
K7GW                        Trojan-Downloader ( 000074d71 )                   20151003  
Avira                       TR/Gendal.6225780                                 20151003  
VBA32                       suspected of Trojan.Downloader.gen.h              20151003  
Rising                      PE:Malware.RDM.41!5.2F[F1]                        20151003  
TrendMicro                  Mal_DLDER                                         20151004  
TrendMicro-HouseCall        Mal_DLDER                                         20151004  
Sophos                      Mal/DownLdr-AC                                    20151003  
Kaspersky                   HEUR:Trojan-Downloader.Win32.Generic              20151004  
Comodo                      Heur.Suspicious                                   20151003  
McAfee                      Generic.dx!625AC05FD47A                           20151004  
Emsisoft                    Gen:Trojan.Heur.RP.cqW@aqIk5pji (B)               20151004  
Ad-Aware                    Gen:Trojan.Heur.RP.cqW@aqIk5pji                   20151004  
BitDefender                 Gen:Trojan.Heur.RP.cqW@aqIk5pji                   20151003  
F-Secure                    Gen:Trojan.Heur.RP.cqW@aqIk5pji                   20151003  
GData                       Gen:Trojan.Heur.RP.cqW@aqIk5pji                   20151003  
MicroWorld-eScan            Gen:Trojan.Heur.RP.cqW@aqIk5pji                   20151003  
Zillya                      Downloader.Small.Win32.47818                      20151003  
AVG                         Downloader.Generic11.BAQU                         20151003  
Symantec                    Downloader                                        20151003  
McAfee-GW-Edition           BehavesLike.Win32.Downloader.nz                   20151003  
Ikarus                      Backdoor.Win32.SuspectCRC                         20151003  
ESET-NOD32                  a variant of Win32/TrojanDownloader.Small         20151003  
```**2. Are there any indications that this file is packed or obfuscated? If so, what are these indicators? If the file is packed, unpack it if possible.**```
Name    Virtual address    Virtual size    Raw size   Entropy     MD5 
.text   4096               1824            4096       3.12        77df9f7ebc4a2bc4bdf2b454d7635aee              
.rdata  8192               978             4096       1.59        d630e1eb49ed821e38202aefef911a39              
.data   12288              332             4096       0.51        d9a3822a7733a76776d8b6e64e364b9d              
.rsrc   16384              16480           20480      0.71        398569177d4d82090d3e1747be560f9a

The PE sections of the file do not immediately suggest that a packer was used.

VirusTotal Packers identified:
  1) Armadillo v1.71  
```**3. When was this program compiled?**```
Compilation timestamp 2019-08-30 22:26:59
```**4. Do any imports hint at this program’s functionality? If so, which imports are they and what do they tell you?**```    
Main Binary:
------------

[+] ADVAPI32.dll
    AdjustTokenPrivileges ----> Indicates privileges are modified somehow.
    LookupPrivilegeValueA
    OpenProcessToken

[+] KERNEL32.dll
    CreateRemoteThread
    MoveFileA
    GetTempPathA
    SizeofResource
    LoadResource -------------> Loads data from the resource section of the executable.
    GetModuleHandleA
    OpenProcess --------------> Creates new processes.
    GetWindowsDirectoryA
    WriteFile ----------------> Modifies files.
    GetCurrentProcess
    CloseHandle
    CreateFileA --------------> Creates new files.
    GetProcAddress
    FindResourceA
    LoadLibraryA -------------> Loads libraries dynamically.
    WinExec ------------------> Executes commands.

--------------------------------------------------------------------------------------------------
    
Resource Binary:
----------------

[+] MSVCRT.dll
    _except_handler3
    __p__fmode
    _adjust_fdiv
    __setusermatherr
    __p__commode
    __p___initenv
    _controlfp
    exit
    _XcptFilter
    __getmainargs
    _snprintf
    _exit
    _stricmp
    _initterm
    __set_app_type
    
[+] KERNEL32.dll
    GetWindowsDirectoryA
    GetTempPathA
    WinExec ------------------> Executes commands.

[+] MSVCRT.dll
    _except_handler3
    __p__fmode
    _initterm
    _adjust_fdiv
    __setusermatherr
    __p__commode
    __p___initenv
    exit
    _XcptFilter
    __getmainargs
    _snprintf
    _controlfp
    _exit
    __set_app_type

[+] urlmon.dll
    URLDownloadToFileA -------> Indicates network communication    
```**5. What host or network-based indicators could be used to identify this malware on infected machines?**```
The following filenames are found in the .data section. I am unaware which of these are system
files and which are filenames designed to look like system files.

Based on the imports of this file, a signature can be made to detemine which of these files are
created or modified after running the binary:
    1) "winlogon.exe"
    2) "sfc_os.dll"
    3) "system32\wupdmgrd.exe"
    4) "winup.exe"
```**6. This file has one resource in the resource section. Use Resource Hacker to examine that resource, and then use it to extract the resource. What can you learn from the resource?**```
The resource is a whole different binary. 

We see references to the following strings in the executable found in the resource:
    1) "http://www.practicalmalwareanalysis.com/updater.exe" - Malicious URL for a network-based indicator
```