# Scenario 04 - No Internet Connectivity

## Reported Problem
A user reported that they could not access the Internet and that websites were not loading.

## Initial Assessment
Questioned user for additional details to establish scope of the issue.
The user's Windows 11 workstation showed as connected to the network, but websites were not loading. The problem had started approximately 30 minutes earlier after working normally.
User indicated that two nearby coworkers were experiencing the same issue, indicating that the problem was not isolated to a single workstation.

## Network Configuration
I checked the workstation's IPv4 configuration using `ipconfig /all`.

The workstation had:
* IPv4 address: `192.168.1.47`
* Subnet mask: `255.255.255.0`
* Default gateway: `192.168.1.1`
* DNS server: `192.168.1.1`

Verified that IPv4 address, default gateway, and DNS are all on the same network.

## Local Connectivity Test
I tested connectivity to the default gateway using `ping 192.168.1.1`.
The ping returned four successful replies with **0% packet loss**.
This confirmed that the workstation could communicate with the local router.

## Internet Connectivity Test
I tested connectivity to an external IP address using: `ping 8.8.8.8`.
The test returned **100% packet loss**.

This indicated that although the workstation could reach the local router, it could not reach an external Internet address.

## DNS Resolution Test
Next I checked for DNS resolution using `ping google.com`.

The hostname resolved to `142.250.72.14`, but the ping received no replies.
This showed that the hostname could be resolved to an IP address, while connectivity to the external destination was still failing.
The DNS resolution could have been successful due to a local cache, so a successful name resolution did not prove that the ping reached the DNS server. 
However, the result was consistent with the existing evidence of an Internet connectivity problem.

## Route Investigation
I used `tracert 8.8.8.8` to examine the path toward the external destination.
* The first hop was the local router and was successful.
* The second hop and subsequent hops timed out.
This confirmed that the workstation could reach the local router, but the traceroute did not provide responses from beyond the local network. Combined with the successful gateway ping and failed external connectivity test, the evidence indicated that the problem was occurring beyond the workstations local network connection. 

## OSI Analysis
The OSI model helped structure the network investigation.

**Layer 2 — Data Link:**
The workstation was connected to the local network and could communicate with the gateway.

**Layer 3 — Network:**
The workstation had a valid IPv4 configuration and could reach the gateway, but traffic could not proceed beyond the router toward the Internet.

The successful gateway ping and failed external ping helped isolate the problem beyond the workstation's local network connection.

## Result
The troubleshooting established that:
* The workstation had a valid IP configuration.
* The workstation could reach the local router.
* DNS could resolve `google.com` to an IP address.
* The workstation could not reach an external Internet address.
* Multiple users were affected.
* Traceroute reached the local router but stopped at the next hop.

These results indicated that the problem was not isolated to the user's workstation and was likely located in the shared network path beyond the local router.

## Resolution / Escalation
As a tier 1, I would not modify the router or WAN configuration without appropriate authorization.
The evidence would be documented and escalated to the network team or appropriate network support for further investigation of the router's WAN connection.

## Lessons Learned
This exercise reinforced the importance of determining the scope of a problem before troubleshooting an individual workstation.
Testing the gateway established that local connectivity was working. Testing an external IP and using traceroute then helped isolate the failure to the network path beyond the local router.
The exercise also demonstrated that a Tier 1 technician does not necessarily need to repair the underlying infrastructure problem. A successful troubleshooting process can instead establish the scope and location of the failure and provide sufficient evidence for escalation.
