# Scenario 05 - Locked Active Directory Account

## Reported Problem
A user reported that they could not log in to their computer because their account was locked.

## Initial Assessment
I began by gathering information from the user to establish the symptoms and determine whether there was an obvious cause for the failed login attempts.

I asked:
* Had the user changed their password recently?
* Was the user certain they were entering the correct password?
* Was Caps Lock or Num Lock enabled?
* Were other users experiencing similar problems?

The user reported that they had not recently changed their password, believed they were using the correct password, and had previously been able to log in successfully.

## Active Directory Investigation
I opened **Active Directory Users and Computers** and asked the user for their username and department so I could locate the correct account.

The user provided:
* Username: `jsmith`
* Department: HR

I navigated to `Organization Name → Accounts → HR` and located the user's account.
I checked the account properties before making any changes.

The account showed:
* **Account is disabled:** No
* **Account is locked out:** Yes
* **Password never expires:** No
* **User must change password at next logon:** No

This confirmed that the account was active but currently locked out.

## Investigation
The account had become locked after reaching the domain's configured account lockout threshold.
However, the underlying reason for the failed authentication attempts had not been established.
Possible causes could include repeated incorrect password entry, an old password being stored on another device or application, or another device repeatedly attempting authentication.
Because the user had not recently changed their password and reported that the password had worked previously, I did not assume that a password reset was required.

## Resolution
After verifying the user's identity according to the Help Desk procedure, I unlocked the `jsmith` account in Active Directory Users and Computers.
Because the account was not disabled and there was no evidence that the password itself needed to be changed, I did not perform an unnecessary password reset.

## Verification
I remained on the call with the user while they attempted to sign in again.
The user successfully logged in using their existing password.
This verified that unlocking the account resolved the immediate login problem.

## Root Cause
The immediate cause of the login failure was that the user's Active Directory account had been locked after reaching the configured account lockout threshold.
The underlying cause of the failed authentication attempts was not definitively established.
The account was successfully unlocked without changing the user's password.

## Lessons Learned
This exercise reinforced the importance of distinguishing between an account being **locked** and an account being **disabled**.
It also demonstrated that a locked account does not automatically require a password reset. The appropriate resolution depends on the evidence gathered during troubleshooting.
Finally, the exercise reinforced the importance of verifying the user's identity before making account changes and confirming that the user can successfully log in before resolving the ticket.
