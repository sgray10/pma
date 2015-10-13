#Chapter 2: Notes

## Steps to running and analyzing malware using VMware:

1. Start with a clean snapshot with no malware running on it.
2. Transfer the malware to the virtual machine.
3. Conduct your analysis on the virtual machine.
4. Take your notes, screenshots, and data from the virtual machine and transfer it to the physical machine.
5. Revert the virtual machine to the clean snapshot.


## Malware Analysis in Virtual Machines

- **Use air-gapped networks**
  - Isolated networks with machines that are disconnected from any other network.
  - Prevent the malware from unwanted spreading.
  - One disadvantage is the lack of an Internet connection.
  - Many pieces of malware depend on a live internet connection for updates, C&C, etc.

- **Physical Machine vs Virtual Machine**
  - Virtual machine can be restored to snapshots or wiped clean. Much easier to remove malware.
  - However, malware can sometimes execute differently on virtual machines.
  
- **Risks of Using VMware for Malware Analysis**
  - Some malware can detect when it is running within a virtual machine.
  - Some malware will execute differently when running on a virtual machine.
  - Vulnerabilities have been found in VMWare allowing host attacks.



## VMWare Network Types

- **Host-Only Networking**
  - Commonly used for malware analysis.
  - Creates a separate private LAN between the host OS and the guest OS.
  - Not connected to the Internet. Seperate network from physical network.

- **Bridged Networking**
  - Allows the virtual machine to be connected to the same network interface as the physical machine.

- **NAT**
  - NAT mode shares the host’s IP connection to the Internet.
  - The host acts like a router and translates all requests from the virtual machine.
  - All requests come from the host’s IP address.

- **Virtual machine team**
  - Requires multiple virtual machines.
  - Machines linked by a LAN but disconnected from the Internet and host machine.
  - The malware is connected to a network, but the network isn’t connected to anything important.
  - When machines are on a team, you will be able to manage their power and network settings together.



## State Management

- **Snapshots**
  - A concept unique to virtual machines.
  - Allow you save a computer’s current state and return to that point later, similar to a Windows restore point.
  - Like a built-in undo feature that saves you the hassle of needing to reinstall your OS.

- **Record/Replay**
  - Records everything that happens so that you can replay the recording at a later time.
  - Record/Replay actually executes the CPU instructions of the OS and programs.
  - Offers 100 percent fidelity. Every instruction that executed during the recording is executed during a replay.
  - If the recording includes a one-in-a-million race condition that you can’t replicate, it will be in the replay.
  - VMWare also has a movie-capture feature that records only the video output.

