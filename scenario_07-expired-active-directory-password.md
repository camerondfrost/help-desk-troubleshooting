# Scenario 07 — Expired Active Directory Password

## Reported Problem
A Sales user reported that they could not log in to their Windows workstation because Windows reported that their password was incorrect.

## Initial Assessment
I first verified the user's identity according to Help Desk procedures.
I gathered the user's username and department and located the account in Active Directory Users and Computers `Organization Name → Accounts → Sales → jsmith`.
I checked the account status before making any changes.
The account was **enabled**.

## Initial Troubleshooting
I asked the user to verify that **Caps Lock** and **Num Lock** were set correctly before attempting to enter the password again.
The user confirmed that Caps Lock was off and Num Lock was on.
The user attempted to log in again, but Windows continued to report that the password was incorrect.
I then asked whether the user had recently changed their password.
The user reported that they had not changed their password recently and had previously been able to log in successfully.

## Active Directory Investigation
I reviewed the user's account settings in Active Directory Users and Computers.
The account's password had **expired**.
This provided an explanation for why the user's existing password was no longer being accepted.

## Resolution
Because the password had expired, I did not perform an unnecessary account unlock or disable/enable operation.
I assisted the user with changing their password according to Help Desk procedures.

## Verification
I remained on the call while the user attempted to log in using the new password.
The user successfully authenticated and logged in to Windows.
This verified that the expired password was the cause of the login problem and that changing the password resolved the issue.

## Root Cause
The user's Active Directory password had expired.
The user had previously been able to authenticate successfully, but the password had reached the configured password expiration period.
Changing the password restored the user's ability to authenticate.

## Lessons Learned
This exercise reinforced the importance of checking account and password status before immediately performing a password reset.
An incorrect-password message does not necessarily mean that the user has forgotten or entered the wrong password. Account and password conditions can also prevent authentication.
The exercise also reinforced the importance of verifying the resolution with the user before closing the ticket.
