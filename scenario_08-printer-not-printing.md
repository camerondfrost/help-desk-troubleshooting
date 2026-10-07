# Scenario 08 - Network Printer Showing Offline

## Reported Problem
A user reported that their network printer was showing as **Offline** and they were unable to print. The printer had been working earlier in the day.

## Initial Assessment
The printer was powered on and displayed no error messages.
Because this was a network printer, I first wanted to determine whether the printer was physically operational and reachable from the workstation before making changes to the Windows printer configuration.

## Investigation

### 1. Verify Printer Power
The printer was confirmed to be:
* Powered on
* Connected to power
* Displaying normally
* Showing no error messages

This established that there was no obvious physical or hardware issue.

### 2. Test Network Connectivity
I pinged the printer from the workstation.

```text
Pinging 192.168.1.50 with 32 bytes of data:
Reply from 192.168.1.50: bytes=32 time=2ms TTL=64
Reply from 192.168.1.50: bytes=32 time=2ms TTL=64
Reply from 192.168.1.50: bytes=32 time=1ms TTL=64
Reply from 192.168.1.50: bytes=32 time=2ms TTL=64

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

The workstation could successfully reach the printer's current IP address.
This indicated that basic network connectivity to the printer was functioning.

### 3. Check the Print Queue
I checked the print queue and found one document stuck in **Printing**.
I cleared the queued document and restarted the Print Spooler service. The queue remained empty, but the printer continued to show as **Offline**.
This indicated that the stuck print job was not the underlying cause, so I continued investigating the printer's network configuration.

### 4. Check the Printer Port
I inspected the printer's configured TCP/IP port.
The Windows printer was configured with:

```text
Port type:          Standard TCP/IP Port
Configured address: 192.168.1.51
```

However, the printer's current IP address was:

```text
192.168.1.50
```

The workstation could reach `192.168.1.50`, but Windows was configured to send print jobs to the outdated address `192.168.1.51`.

## Root Cause
The confirmed problem was a **misconfigured Windows printer port**.
The printer's current IP address was `192.168.1.50`, while the Windows printer configuration still referenced the previous address `192.168.1.51`.
The printer was using DHCP, so the address may have changed after the original address was assigned. However, the exact reason for the IP address change was not independently determined.

## Resolution
The printer's network configuration should be managed so that its address remains consistent.

A recommended approach would be to:
1. Create a DHCP reservation for the printer's MAC address.
2. Reserve `192.168.1.50` for the printer.
3. Update the Windows Standard TCP/IP printer port to `192.168.1.50`.
4. Clear any remaining print jobs.
5. Send a test document to the printer.

## Verification
Successful resolution would be verified by confirming that:
* The printer shows as Online.
* The Windows printer port points to `192.168.1.50`.
* A test document successfully prints.
* The printer remains reachable at its assigned address.

## Lessons Learned
This scenario demonstrated that successful network connectivity does not necessarily mean that an application is correctly configured to communicate with a device.
A successful ping established that the printer was reachable at its current IP address, but it did not establish that Windows was configured to use that same address.
For network printers, both the printer's current network address and the Windows printer port configuration need to point to the same device.
DHCP reservations can help prevent this type of problem by giving network printers a consistent IP address while still allowing the DHCP server to centrally manage network addressing.
