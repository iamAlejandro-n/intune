# 06 - Conditional Access

## Objective

Require a compliant device to access Microsoft 365, and prove that compliant devices are allowed and non-compliant devices are blocked.

## Prerequisites

- Entra ID P1 on the targeted users
- Compliance policy in place ([compliance](03-compliance.md))
- Break-glass admin account created ([tenant setup](01-tenant-setup.md))

## Policy design

| Setting | Value |
|---------|-------|
| Name | `CA-Require-Compliant-Device` |
| Users | Include `Intune-Pilot` |
| Exclude | Break-glass account and your own admin account |
| Target resources | Office 365 (or Exchange Online) |
| Grant | Require device to be marked as compliant |
| State | **Report-only** first, then On |

## Steps

1. Entra admin center > **Protection > Conditional Access > Create new policy**.
2. Configure the design above and set state to **Report-only**.
3. Sign in as `pilot` from the compliant device and from a non-enrolled browser.
4. Review **Sign-in logs > Conditional Access** to see what would have been blocked.
5. When results match expectations, change the state to **On**.
6. Repeat the sign-in tests.

## Test matrix

| Scenario | Expected |
|----------|----------|
| `pilot` on compliant device | Allowed |
| `pilot` on non-compliant device | Blocked |
| `pilot` on unenrolled device | Blocked |
| `control` (not targeted) | Allowed |
| Break-glass account | Allowed |

## Verification

- [ ] Report-only results reviewed
- [ ] Policy turned On
- [ ] Allowed and blocked cases confirmed in sign-in logs
- [ ] Policy JSON exported (sanitized) to `policies/conditional-access/`

## Screenshots

- `ca-policy-settings.png`
- `ca-signin-allowed.png`
- `ca-signin-blocked.png`

## Safety notes

- Never turn the policy on without an excluded admin account.
- Keep report-only until you have read the logs.
- Have a way to disable the policy from the break-glass account.

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Compliant device still blocked | Compliance state not yet reported | Sync and wait |
| Policy has no effect | Wrong target app or user | Recheck assignments |
| Locked out | No exclusion | Use the break-glass account |

## Notes and issues

- *(Record anything unexpected.)*
