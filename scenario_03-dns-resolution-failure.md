# Scenario 03 - DNS Resolution Failure

## Reported Problem
While testing DNS resolution in the Windows 11 client, `nslookup` reported DNS request timeouts when querying the `LAB.local` domain.

## Initial Assessment
The `LAB.local` domain was hosted by the Windows Server 2022 Domain Controller, which also provided DNS services for the lab.
Because `nslookup` was reporting DNS request timeouts, I needed to determine whether the problem was related to DNS resolution itself, network connectivity between the client and Domain Controller, or the DNS service on the server.

## Network Connectivity Investigation
I first verified that the Windows 11 client could communicate with the Domain Controller.
From the client, I tested connectivity to the Domain Controller's IPv4 address, `192.168.56.10`, using `ping`.
The client received replies with **0% packet loss**, demonstrating that basic network connectivity between the client and Domain Controller was functioning.

## DNS Service Investigation
Because basic network connectivity between the client and Domain Controller was functioning, I investigated whether the DNS service on DC01 was running and accepting DNS traffic.
I verified that the DNS service was running and that DC01 was listening for DNS traffic on port 53.
I also reviewed the Windows Firewall configuration and confirmed that inbound DNS traffic was allowed over both TCP and UDP port 53.

## DNS Resolution Test
I tested DNS resolution for `LAB.local` from the Windows 11 client using `Resolve-DnsName` and specified the Domain Controller's IPv4 address as the DNS server.
The query successfully returned `LAB.local` with the address `192.168.56.10`.
This demonstrated that the client could successfully resolve the domain through the Domain Controller's DNS service, despite the timeout behavior reported by `nslookup`.

## `nslookup` Investigation
Although `Resolve-DnsName` successfully resolved `LAB.local`, `nslookup` continued to report DNS request timeouts.
I checked the DNS server being used by `nslookup` and found that it was using the IPv6 loopback address, `::1`, as its default DNS server.
I then tested the same `LAB.local` query using the IPv4 loopback address, `127.0.0.1`, instead.

## Test Result
When I queried `LAB.local` using `127.0.0.1` as the DNS server, `nslookup` returned the expected result without the same timeout behavior.
The response resolved `LAB.local` to `192.168.56.10`.
This demonstrated that the DNS service could successfully answer the query when accessed through the IPv4 loopback address.

## Verification
I verified the DNS configuration by successfully resolving `LAB.local` from the Windows 11 client using `Resolve-DnsName` against the Domain Controller at `192.168.56.10`.
I also verified that the Domain Controller was reachable on TCP port 53 and that its DNS service was listening for DNS traffic.
These tests confirmed that the Domain Controller's DNS service was functioning and that the client could successfully communicate with it for DNS resolution.

## Root Cause
The DNS service on DC01 was functioning correctly, and the Windows 11 client had working network connectivity to the Domain Controller.
The timeout behavior was isolated to `nslookup` when it used the IPv6 loopback address, `::1`, as its default DNS server. When the query was directed to the IPv4 loopback address, `127.0.0.1`, `nslookup` returned the expected result.
The testing demonstrated that the DNS resolution failure was specific to the way `nslookup` was accessing the DNS service. The underlying reason for the different behavior between `::1` and `127.0.0.1` was not independently determined.


## Lessons Learned
This troubleshooting exercise demonstrated the importance of testing multiple components before concluding that a service is failing.
Basic network connectivity, the DNS service, port 53, and firewall rules were all verified before investigating the behavior of the specific DNS utility.
The exercise also reinforced the value of using more than one diagnostic tool. `Resolve-DnsName` successfully resolved the domain even while `nslookup` reported timeouts, which helped narrow the problem to the way `nslookup` was accessing the DNS service rather than DNS being completely unavailable.
Finally, the exercise demonstrated the importance of distinguishing between a confirmed finding and an unresolved underlying cause.
