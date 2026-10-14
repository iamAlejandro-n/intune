# 01 - Tenant Setup

## Objective

Prepare a clean test tenant with the licenses, users, groups and safety nets needed for the rest of the lab.

## Prerequisites

- A test tenant (do not use production)
- Global Administrator access
- Either Microsoft 365 E3 (includes Intune Plan 1 and Entra ID P1) or an Intune trial plus an EMS trial

## Steps

### 1. Confirm licenses

1. Microsoft 365 admin center → **Billing → Licenses**.
2. Confirm you have Intune Plan 1 and Entra ID P1 (directly, or through Microsoft 365 E3 / EMS).
3. Record the license names below.

| License | Present? |
|---------|----------|
| Microsoft Intune Plan 1 | |
| Microsoft Entra ID P1 | |

### 2. Create test users

| User | Purpose |
|------|---------|
| `pilot` | Enrolled device user, targeted by policies |
| `control` | Not targeted, used for comparison |
| `breakglass` | Emergency admin, excluded from Conditional Access |

Assign licenses to `pilot` and `control`.

### 3. Create groups

1. Entra admin center → **Groups → New group**.
2. Create a security group `Intune-Pilot` and add the `pilot` user.

### 4. Set MDM user scope

1. Entra admin center → **Mobility (MDM and MAM) → Microsoft Intune**.
2. Set **MDM user scope** to *Some* and select `Intune-Pilot`, or *All* for a lab-only tenant.

### 5. Secure the break-glass account

- Use a long random password stored offline.
- Assign Global Administrator.
- Exclude it from every Conditional Access policy.

## Verification

- [ ] Licenses visible on test users
- [ ] `Intune-Pilot` group contains `pilot`
- [ ] MDM user scope set
- [ ] Break-glass account created and documented offline

## Screenshots

Add sanitized screenshots to `images/`:

- `licenses.png`
- `groups.png`
- `mdm-scope.png`

## Notes and issues

- *(Record anything unexpected.)*
