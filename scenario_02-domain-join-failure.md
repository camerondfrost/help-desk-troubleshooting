# Scenario 02 - Windows 11 Domain Join Failure

## Reported Problem
While attempting to join the Windows 11 client to the Active Directory domain, I encountered a two-part problem. First, the option to join a domain was initially unavailable.
Second, once the domain join option became available, entering the domain name did not successfully connect the client to the Domain Controller.

## Initial Assessment
The task was to join the Windows 11 client to the `LAB.local` Active Directory domain.

The initial problem was that the option to join a domain was unavailable. My first step was to investigate on the Windows client to determine why the domain join was unavailable.

After the domain join option became available, entering `LAB.local` still failed. At that point, the investigation shifted toward determining whether the client could communicate with the Domain Controller.

## Investigation — Windows Edition
I first checked the installed Windows System Information to determine the edition.
The Windows 11 ISO that was used for the virtual machine was a multi-edition installation source, but the client had automatically installed **Windows 11 Home** by default.
Windows 11 Home does not support the ability to join an Active Directory domain, explaining why the domain join option was unavailable.

## Test
I disconnected the Windows 11 client from the Internet so it so Microsoft could not perform the automatic check for OEM key and opened **Settings → System → Activation**.
I entered the Windows 11 Pro generic product key provided for the lab and restarted the client.

## Result
After restarting, Windows 11 was upgraded from Home to Pro. The option to join the computer to a domain became available.

## Verification
I confirmed that the Windows client now provided the domain join option, allowing me to proceed with attempting to connect it to `LAB.local`.

## Network Investigation
After resolving the Windows edition issue, the domain join option became available, but entering `LAB.local` still failed.
I first tested whether the Windows client could communicate with the Domain Controller. From the Windows client, I attempted to ping the server, but the request timed out.
the failed ping indicated that network connectivity should be investigated before continuing to troubleshoot the domain join.

## Network Configuration
I reviewed the VirtualBox recommended network configuration after the client was unable to reach the Domain Controller.
The Windows 11 client was configured with a Host-Only Adapter. The Windows Server was configured with both a Host-Only Adapter and a NAT Adapter.
Because the client could not successfully ping the Domain Controller, I continued investigating the VirtualBox network configuration.

## Network Configuration Test
After the initial connectivity tests failed, I researched the issue using ChatGPT as a troubleshooting resource to determine an alternate configuration to test. 
Based on its suggestion, I removed the additional NAT configuration from the server and changed the network adapters from Host-Only to Internal Network on both virtual machines. 
I configured both virtual machines to the same Internal network `labnet`.
I then restarted both virtual machines and tested connectivity from the Windows 11 client by pinging the domain controller. 

## Test Result
After changing both virtual machines to the `labnet` Internal Network, the Windows 11 client was able to successfully ping the Domain Controller.
The ping test returned replies with **0% packet loss**, confirming that the client and server could now communicate over the virtual network.

## Domain Join Verification
With network connectivity restored, I attempted to join the Windows 11 client to the `LAB.local` domain again.
The client successfully connected to the Domain Controller and joined the `LAB.local` domain.
This confirmed that the network connectivity issue had been preventing the client from completing the domain join.

## OSI Analysis
The network portion of the troubleshooting process was approached using the OSI model.

**Layer 2 — Data Link:**
The VirtualBox network configuration was examined to determine whether both virtual machines were connected to the same virtual network.

**Layer 3 — Network:**
IP connectivity was tested by pinging the Domain Controller from the Windows 11 client. The initial test timed out. After changing both machines to the same Internal Network, the ping succeeded with 0% packet loss.

**Layer 7 — Application:**
The original failure occurred when attempting to join the Active Directory domain. Once Layer 3 connectivity was restored, the domain join completed successfully.

The testing showed that the domain join failure was assoctiated with the clients inability to communicate with the Domain Controller over the virtual network.

## Root Cause
Two separate issues prevented the Windows 11 client from completeing the domain join.

The first issue was the Windows edition. The client had Windows 11 Home installed, which does not support joining an Active Directory domain. Upgrading the client to Windows 11 Pro made the domain join option available.

The second issue was network connectivity. The Windows 11 client could not initially communicate with the Domain Controller over the configured VirtualBox network. Changing both virtual machines to the same Internal Network restored connectivity and allowed the domain join to complete successfully.

The testing demonstrated tat the network configuration change restored communication between the client and Domain Controller and that the domain join succeeded afterward. 
The specific mechanism that caused the original Host-Only connectivity problem was not independently determined.

## Lessons Learned
This troubleshooting exercise demonstrated the importance of separating a problem into individual issues rather than assuming there is a single cause.

The first issue was identified by examining the Windows client's capabilities. The second required testing network connectivity and using the OSI model to narrow the problem to the network layers.

The exercise reinforced the importance of documenting what was tested, what changed, and what was confirmed, while distinguishing between a changed that resolved a problem and a root cause that has ben definitively established. 
