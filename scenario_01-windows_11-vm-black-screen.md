# Scenario 01 - Windows 11 VM Black Screen

## Reported Problem
While setting up the Windows 11 client virtual machine in VirtualBox, the VM started but the display remained small and black instead of displaying the expected Windows setup normally.

## Initial Assessment
The physical monitor and host system display were functioning normally, so the problem appeared to be isolated to the virtual machine rather than the physical display hardware.
Because the VM was starting but was not displaying Windows normally, I focused the investigation on the virtual machine configuration.

## Research and Investigation
I was unfamiliar with the specific VirtualBox display behavior, so I researched the symptoms and reviewed the VM's configuration.
Based on the symptoms and the existing VM configuration, CPU allocation was identified as a possible contributing factor. Rather than assuming this was the cause, I tested whether changing the CPU allocation would affect the problem.

## Test
In the VirtualBox system settings, I increased the VM's assigned processors from 2 to 3.
I then restarted the virtual machine to determine whether the change affected the display problem.

## Result
The Windows 11 virtual machine booted normally and displayed the Windows setup correctly.
I was then able to use the View menu to select Full Screen and expand the display to fill the monitor.

## Verification
I restarted the VM again to confirm that the configuration change continued to allow Windows 11 to boot and display normally.

## Root Cause
The problem was associated with the VM's CPU allocation. The VM was configured with 2 CPUs and exhibited the black-screen behavior, while increasing the allocation to 3 CPUs allowed it to boot and display normally.
However, the specific underlying cause of the black-screen behavior was not independently determined.

## Lessons Learned
This troubleshooting exercise reinforced the importance of first determining whether a problem is isolated to the physical system or to a virtual environment.
Because the host display was functioning normally, the investigation focused on the VM rather than the physical display hardware.
The exercise also demonstrated the importance of testing a suspected configuration change and verifying the result rather than assuming the change resolved the problem.
Although increasing the CPU allocation resolved the issue, the exact underlying cause was not independently determined.

The exercise also demonstrated the value of testing a suspected configuration change and verifying the result rather than assuming the change resolved the problem.
Although increasing the CPU allocation resolved the issue, the specific underlying cause was not independently determined.
