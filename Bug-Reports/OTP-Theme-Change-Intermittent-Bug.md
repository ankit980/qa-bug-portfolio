# Intermittent OTP Input Issue After Theme Change

## Summary

An intermittent issue was observed in the OTP verification screen where editing an entered OTP caused one or more digits to disappear after switching the application's theme.

> **Note:** This bug report has been recreated from a real production issue. Product names and implementation details have been removed to protect confidential information.

---

## Environment

- Platform: Android
- Feature: User Authentication
- Build: Production
- Theme: Light / Dark

---

## Severity

High

## Priority

High

---

## Preconditions

- User account exists.
- OTP authentication is enabled.
- Application supports Light and Dark themes.

---

## Steps to Reproduce

1. Launch the application.
2. Log in while the application is using the **Light Theme**.
3. Log out.
4. Change the application theme to **Dark Theme**.
5. Start the login process again.
6. Enter the OTP received.
7. Delete one OTP digit using Backspace.
8. Enter the new digit.

---

## Expected Result

The edited OTP digit should be updated correctly, all digits should remain visible, and the OTP should be accepted.

---

## Actual Result

After editing an OTP digit, one or more digits occasionally disappeared, focus sometimes moved unexpectedly between OTP fields, and the OTP could not always be submitted even though the correct value had been entered.

---

## Why the Issue Was Intermittent

The issue did not occur during every login attempt.

It was observed only after:
- changing the application theme,
- logging out and logging in again,
- editing an already-entered OTP.

Because these conditions had to occur together, reproducing the issue consistently was difficult.

---

## Investigation

During debugging I:

- Reproduced the issue multiple times using different theme combinations.
- Verified that the backend OTP API returned successful responses.
- Confirmed the issue was limited to the client-side OTP input component.
- Shared reproducible steps and observations with the development team.

---

## Root Cause

The issue was traced to the OTP input component being recreated after a theme change without preserving its internal state correctly.

When a user edited an OTP digit, the component incorrectly handled the input state, causing previously entered digits to disappear and resulting in inconsistent OTP validation.

---

## Resolution

The OTP input component was updated to preserve its state correctly after theme changes. Input handling was corrected, and regression testing confirmed that the issue no longer occurred.

---

## Regression Testing

Regression testing covered:

- Login with Light Theme
- Login with Dark Theme
- Light → Dark theme switch
- Dark → Light theme switch
- OTP editing
- OTP auto-fill
- OTP paste
- Invalid OTP
- Expired OTP
- Multiple login/logout cycles

**Status:** Fixed and verified.