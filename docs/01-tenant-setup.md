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
| Microsoft Intune Plan 1 |yes |
| Microsoft Entra ID P1 |yes |

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

### Optional: device group

| Group | Type | Members | Purpose |
|-------|------|---------|---------|
| `Intune-Pilot-Devices` | Security, Assigned | Test device | Target the device regardless of who signs in, and test device-targeted assignments (for example an app installed per device rather than per user) |

### 4. Set MDM user scope

1. in the Intune admin center, the Windows automatic enrollment page (Devices → Enrollment → Windows → Automatic Enrollment)
2. Set MDM user scope to Some, then select Intune-Pilot

### 5. Secure the break-glass account

- Use a long random password stored offline.
- Assign Global Administrator.
- Exclude it from every Conditional Access policy.

## Verification

- [ ] Licenses visible on test users
- [y] `Intune-Pilot` group contains `pilot`
- [ ] MDM user scope set
- [y] Break-glass account created and documented offline

## Screenshots

Add sanitized screenshots to `images/`:

- `licenses.png`
- `groups.png`<img width="242" height="181" alt="groups" src="https://github.com/user-attachments/assets/19ec1502-5af0-44c2-a13b-c9f5bc65699f" />

- `mdm-scope.png`

## Notes and issues

- *(Record anything unexpected.)*
