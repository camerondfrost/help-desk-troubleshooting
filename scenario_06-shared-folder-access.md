# Scenario 06 - Shared Folder Access

## Reported Problem
A user reported that they could not access the HR shared folder and received an **Access Denied** message.

## Initial Assessment
I first reproduced the reported problem from the user's workstation by attempting to access the HR shared folder.
The Access Denied message was reproduced successfully.
I then determined whether the issue affected other users. Other HR employees were able to access the shared folder, indicating that the problem was isolated to the individual user's access rather than the shared folder as a whole.

## Network Connectivity
I tested connectivity between the user's workstation and the Domain Controller using `ping DC01`.
The test returned successful replies with **0% packet loss**.
This confirmed that the user's workstation could communicate with the Domain Controller, so I continued investigating the user's access permissions.

## Active Directory Investigation
I located the user's account in **Active Directory Users and Computers** `Feywild Farms → Accounts → HR → jsmith`
I checked the user's group membership and found that `jsmith` was not a member of the **HR** security group.
I also checked the **HR** security group's membership and confirmed that `jsmith` was not listed.

## Share Permission Investigation
I checked the shared folder configuration on the Domain Controller `C:\HR-Shared → Properties → Sharing → Advanced Sharing → Permissions`
The **HR** security group was configured with **Read** and **Change** permissions.
This confirmed that the share was configured to grant access to members of the HR group.

## NTFS Permission Investigation
I checked the folder's NTFS permissions `C:\HR-Shared → Properties → Security`
The **HR** security group had **Read** and **Modify** permissions.
This confirmed that the folder's NTFS permissions were also configured to grant the appropriate access to members of the HR group.

## Root Cause
The user's Active Directory account was not a member of the **HR** security group.
The HR security group was configured with the required share and NTFS permissions, but `jsmith` was not a member of the group and therefore did not receive those permissions.

## Resolution
After verifying the user's identity and authorization according to Help Desk procedures, I added `jsmith` to the **HR** security group.
The user then signed out and signed back in so the updated group membership would be reflected in their logon session.

## Verification
I attempted to access the HR shared folder again from the user's workstation.
The user was able to open the shared folder successfully.
This verified that restoring the user's membership in the HR security group resolved the access problem.

## Lessons Learned
This exercise reinforced the importance of determining the scope of an access problem before making changes.
Because other HR users could access the share and the user's workstation could communicate with the Domain Controller, the investigation could be narrowed to the user's authorization.
Checking both the user's group membership and the permissions assigned to the HR security group helped establish the relationship between the user's account and the resources they were attempting to access.
The exercise also reinforced the distinction between **Active Directory group membership**, **share permissions**, and **NTFS permissions** when troubleshooting access to a network share.
