# 07 - Testing Results

## Objective

Record the outcome of every test case with evidence, so another admin can verify the lab works.

## Test environment

| Item | Value |
|------|-------|
| Date tested | `<YYYY-MM-DD>` |
| Device | `<VM or physical, OS build>` |
| Enrollment method | `<registered / joined / Autopilot>` |
| Licenses | `<license set>` |

## Results

| ID | Test | Expected | Result | Evidence |
|----|------|----------|--------|----------|
| T1 | Enroll device | Appears in Devices > All devices | ✅ / ❌ | `images/device-in-intune.png` |
| T2 | Compliance policy applied | Compliant, or a clear failure reason | ✅ / ❌ | `images/compliance-device-status.png` |
| T3 | Configuration profile applied | Setting visible, status Succeeded | ✅ / ❌ | `images/config-profile-status.png` |
| T4 | Required app deployment | Installs silently | ✅ / ❌ | `images/app-install-status.png` |
| T5 | Conditional Access, compliant device | Sign-in succeeds | ✅ / ❌ | `images/ca-signin-allowed.png` |
| T6 | Conditional Access, non-compliant device | Sign-in blocked | ✅ / ❌ | `images/ca-signin-blocked.png` |
| T7 | Remote actions | Sync and restart work; retire removes company data | ✅ / ❌ | `images/remote-actions.png` |

## Detailed notes per test

### T1 - Enrollment
- Steps taken:
- Observed result:
- Issues:

### T2 - Compliance
- Steps taken:
- Observed result:
- Issues:

### T3 - Configuration
- Steps taken:
- Observed result:
- Issues:

### T4 - App deployment
- Steps taken:
- Observed result:
- Issues:

### T5 and T6 - Conditional Access
- Steps taken:
- Observed result:
- Issues:

### T7 - Remote actions
- Steps taken:
- Observed result:
- Issues:

## Defects and fixes

| Issue | Cause | Fix |
|-------|-------|-----|
| | | |

## Summary

- Tests passed: `<n>` of 7
- Biggest surprise:
- What I would change:

