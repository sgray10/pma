#Chapter 3: Notes

## Basic Dynamic Analysis

- **Useful Programs**
  - Sandbox Apps (Norman SandBox, GFI Sandbox, Anubis, Joe Sandbox, ThreatExpert, BitBlaze, etc)
  - procmon (Process Monitor)
  - Process Explorer
  - Dependency Walker
  - Regshot
  - ApateDNS
  - Netcat
  - Wireshark
  - INetSim

- **Dynamic Analysis**
  - Any examination performed after executing malware.
  - The second step in the malware analysis process after basic static analysis has reached a dead end
  - Can involve monitoring malware as it runs or examining the system after the malware has executed.
  - Lets you observe the malware’s true functionality
  - Dynamic analysis can put your network and system at risk.



## Sandboxes

- **Using a Malware Sandbox**
  - Security mechanism for running untrusted programs in a safe environment without fear of harming “real” systems.
  - Comprise virtualized environments that often simulate network services to ensure malware will function normally.
  - Norman SandBox and GFI Sandbox are the most popular among computer-security professionals.
  - Provide easy-to-understand output and are great for initial triage.
  - Require you to submit your malware to the sandbox websites.

- **GFI Sandbox Sample Results**
  - The Analysis Summary section lists static analysis information and a high-level overview of the dynamic analysis results.
  - The File Activity section lists files that are opened, created, or deleted for each process impacted by the malware.
  - The Created Mutexes section lists mutexes created by the malware.
  - The Registry Activity section lists changes to the registry.
  - The Network Activity section includes network activity spawned by the malware.
  - The VirusTotal Results section lists the results of a VirusTotal scan of the malware.  

- **Sandbox Drawback**
  - The sandbox simply runs the executable, without command-line options.
  - It will not execute any code that runs only when an option is provided.
  - The sandbox may not wait long enough to record all events.
  - The malware might stop running or behave differently if it detects it is in a virtual machine.
  - The sandbox environment OS may not be correct for the malware.



## Running Malware

- **Getting a malware DLL to run**
  - The majority of malware you will encounter are EXEs and DLLs.
  - It can be tricky to launch malicious DLLs.
  - The program rundll32.exe is included with all modern versions of Windows.
  - To run a DLL: ```C:\>rundll32.exe DLLname, EXPORT arguments```
  - The EXPORT value must be a function name or ordinal selected from the exported function table in the DLL
  - You can use a tool such as PEview or PE Explorer to view the Export table.
  - Prepended the EXPORT with the # character when launching using an ordinal: ```C:\>rundll32.exe xyzzy.dll, #5```

- **Properties of malicious DLLs**
  - Malicious DLLs frequently run most of their code in DLLMain (called from the DLL entry point).
  - DLLMain is executed whenever the DLL is loaded.
  - You can often get information dynamically by forcing the DLL to load using rundll32.exe.
  - You can turn a DLL into an EXE by modifying the PE header and changing its extension to force Windows to load the DLL as an EXE

- **Converting a DLL to an EXE**
  - To modify the PE header, wipe the IMAGE_FILE_DLL (0x2000) flag from the Characteristics field in the IMAGE_FILE_HEADER.
  - While this change won’t run any imported functions, it will run the DLLMain method.
  - It may cause the malware to crash or terminate unexpectedly.
  - As long as your changes cause the malware to execute its malicious payload, you can collect information for your analysis.



## Monitoring Processes

- **Process Monitor (procmon)**
  - Advanced monitoring tool for Windows.
  - Provides a way to monitor certain registry, file system, network, process, system call, and thread activity.
  - It combines and enhances the functionality of two legacy tools: FileMon and RegMon.
  - Very useful, but it doesn’t capture everything.

- **procmon Filters**
  - Can filter on one executable running on the system, useful for malware analysis.
  - Can filter on individual system calls such as RegSetValue, CreateFile, WriteFile, or other suspicious/destructive calls.
  - The most important filters for malware analysis are Process Name, Operation, and Detail.
  - If the malware extracted another executable and ran it, that information is still captured.
  - If you see any malware extracted, change the filter to display the extracted name, and then click Apply.
  - Procmon provides helpful automatic filters on its toolbar:
    - Registry - By examining registry operations, you can tell how a piece of malware installs itself in the registry.
    - File system - Exploring file system interaction can show all files that the malware creates or configuration files it uses.
    - Process Activity - Investigating process activity can tell you whether the malware spawned additional processes.
    - Network - Identifying network connections can show you any ports on which the malware is listening.
  - If your malware runs at boot time, use procmon’s boot logging options to install procmon as a startup driver to capture startup events.

- **Process Explorer** 
  - Extremely powerful task manager that should be running when you are performing dynamic analysis.
  - Lists active processes, DLLs loaded by a process, various Process Properties, and overall system information.
  - Use it to kill a process, log out users, and launch and validate processes.
  - Shows all handles held by the process, including file handles, mutexes, events, etc.
  - Can view detailed properties by double-clicking a process:
    - The Threads tab shows all active threads,
    - The TCP/IP tab displays active connections or ports on which the process is listening
    - The Image tab (opened in the figure) shows the path on disk to the executable.

- **Process Explorer 'Verify' Function**
  - Verify button found on the Image tab of Process Properties.
  - Malware often replaces authentic Windows files with its own in an attempt to hide.
  - Microsoft uses digital signatures for most of its core executables.
  - Process Explorer verifies that a signature is valid, 
  - You can be sure that the file is actually the executable from Microsoft.
  - The Verify button verifies the EXE file on disk, but not every DLL loaded during runtime.
  - Verifies the image on disk rather than in memory, and it is useless if an attacker uses process replacement.

- **Process Replacement**
  - Running a process on the system and overwriting its memory space with a malicious executable.
  - Provides the malware with the same privileges as the process it is replacing.
  - The image in memory will differ from the image on disk.
  - One way to recognize process replacement is to use the Strings tab in the Process Properties window.
  - Compare the strings contained in the disk executable (image) against the strings in memory for that same executable.
  - If the two string listings are drastically different, process replacement may have occurred.



## Monitoring the Registry

- **Regshot**
  - Open source registry comparison tool that allows you to take and compare two registry snapshots.
  - Take registry snapshots before and after running malware and compare the two versions.



## Monitoring/Faking a Network

- **DNS Spoofing with ApateDNS**
  - Free tool from Mandiant.
  - The quickest way to see DNS requests made by malware.
  - Spoofs DNS responses to a user-specified IP address by listening on UDP port 53 on the local machine.
  - Displays the hexadecimal and ASCII results of all requests it receives.
  - Malware will often loop through the different domains it has stored if the first or second domains are not found.
  - You can catch additional domains used by a malware sample through the use of the nonexistent domain (NXDOMAIN) option.

- **Monitoring with Netcat**
  - “TCP/IP Swiss Army knife”
  - Inbound and outbound connections for port scanning, tunneling, proxying, port forwarding, and much more.
  - In listen mode, Netcat acts as a server, while in connect mode it acts as a client.

- **Packet Sniffing with Wireshark**
  - Open source packet capture tool that intercepts and logs network traffic.
  - Provides visualization, packet-stream analysis, and in-depth analysis of individual packets.
  - Used to analyze internal networks and network usage, debug application issues, and study protocols in action.
  - Used to sniff passwords, reverse-engineer network protocols, steal sensitive information, and listen in on network traffic.
  - Help you to understand how malware is performing network communication by sniffing packets as the malware communicates.

- **Using INetSim**
  - Free, Linux-based software suite for simulating common Internet services.
  - Emulates services such as HTTP, HTTPS, FTP, IRC, DNS, SMTP, and others.
  - INetSim can serve almost any file requested on HTTP and HTTPS.
  - If malware requests a JPEG from a website, INetSim will respond with a properly formatted JPEG.
  - Can also record all inbound requests and connections.
  - Use it to record all ports to which the malware connects and the corresponding data that is sent.



## Basic Dynamic Tools in Practice

#### Common setup of tools:

1. Running procmon and setting a filter on the malware executable name and clearing out all events just before running.
2. Starting Process Explorer.
3. Gathering a first snapshot of the registry using Regshot.
4. Setting up your virtual network to your liking using INetSim and ApateDNS.
5. Setting up network traffic logging using Wireshark.

