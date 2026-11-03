# 01 - Tenant Setup

## Objective

Prepare a clean test tenant with the license, user, group and safety nets needed for the rest of the lab.

## Prerequisites

- A test tenant (do not use production)
- Global Administrator access
- Microsoft 365 E3 trial (includes Intune Plan 1 and Entra ID P1)

## Design decisions

| Decision | Choice | Reason |
|----------|--------|--------|
| License | Microsoft 365 E3 trial, `<n>` seats, ends `<date>` | Includes Intune Plan 1 and Entra ID P1, so nothing else is needed |
| Who is licensed | `pilot`, through the `Intune-Pilot` group | Only the enrolled, targeted user needs Intune and Entra ID P1 |
| Admin and break-glass accounts | No license | Admin access comes from a role, not a license |
| `Intune-Pilot` group type | User (security) group | Conditional Access targets users and groups of users, not device groups |
| Control user | Licensed directly with the E3 trial, not in `Intune-Pilot` | Negative control: can open real Microsoft 365 apps, but is never enrolled or targeted |

## Steps

### 1. Confirm the trial license

1. Microsoft 365 admin center → **Billing → Licenses**.
2. Confirm Microsoft 365 E3 (trial) is listed, and record the number of seats and the trial end date.
3. Click it and check that Intune Plan 1 and Entra ID P1 appear among the included services.
4. Under **Billing → Your products**, check whether a payment method is attached and whether the trial converts to a paid subscription when it ends. Cancel before the end date if you do not want to be charged.

| License | Present? | Source | Seats | Expires |
|---------|----------|--------|-------|---------|
| Microsoft Intune Plan 1 | | Microsoft 365 E3 trial | | |
| Microsoft Entra ID P1 | | Microsoft 365 E3 trial | | |

A trial ends on a fixed date and usually cannot be restarted in the same tenant. Plan to finish enrollment, compliance, configuration, apps and Conditional Access before it ends, and capture screenshots while the license is active.

### 2. Create the pilot user

1. Microsoft 365 admin center → **Users → Active users → Add a user**, or Entra admin center → **Users → New user**.
2. Display name: `Pilot User`. Username: `pilot@<tenant>.onmicrosoft.com`.
3. Set **Usage location** (for example, United States). Licenses cannot be assigned to a group member without it.
4. Do not assign a license directly. The group will provide it.

Use generic names only. Do not use a real name or personal email in a public repo.

### 3. Create the group and add the user

1. Entra admin center → **Groups → All groups → New group**.
2. Group type: **Security**. Name: `Intune-Pilot`. Membership type: **Assigned**.
3. Add `pilot` as a member.

This group will also be the assignment target for compliance policies, configuration profiles, apps and the Conditional Access policy.

### 4. Assign the license to the group

1. Entra admin center → **Billing → Licenses → All products**.
2. Select **Microsoft 365 E3** → **Licensed groups → Assign**.
3. Choose `Intune-Pilot` and save.
4. Wait a few minutes, then open the user and confirm the license shows as assigned.

If the license does not appear, check the group's license status for errors (a missing usage location is the most common cause) and use **Reprocess**.

Notes on this approach:

- The trial gives you several seats, so the group is not limited to one member. Keep it to `pilot` anyway, because every member also receives the policies.
- Do not put `control`, the admin account or the break-glass account in `Intune-Pilot`, even though seats are available. If you want `control` licensed, assign the license to that user directly.
- To move the license to another user later, change group membership instead of editing licenses.

### 5. Set the MDM user scope

1. Entra admin center → **Mobility (MDM and MAM) → Microsoft Intune**.
2. Set **MDM user scope** to *Some* and select `Intune-Pilot`, or *All* for a lab-only tenant.
3. Leave the MAM user scope at *None* unless you are testing app protection.

### 6. Secure the break-glass account

- Create `breakglass@<tenant>.onmicrosoft.com` with a long random password stored offline.
- Assign Global Administrator.
- Exclude it from every Conditional Access policy.
- Do not license it or add it to `Intune-Pilot`.

### 7. Optional: control user

- Create `control@<tenant>.onmicrosoft.com`, not in `Intune-Pilot`.
- Used only to show that a non-targeted user is not affected (sign-in log shows Conditional Access as Not applied).
- Set a **Usage location**, then assign the Microsoft 365 E3 trial license **directly** to this user (Microsoft 365 admin center → Users → Active users → `control` → Licenses and apps). Do not add it to `Intune-Pilot`.
- Having a license lets it open Outlook on the web or Teams, so the "allowed" test uses a real Microsoft 365 app.
- Keep the MDM user scope on *Some* (`Intune-Pilot`). With *All*, a licensed `control` signing in on a device could auto-enroll in Intune.
- Skip this step if you decide not to use a control user, and remove it from the later pages.

### Optional: device group you can tie to the device itself.
if you dont want to tie mdm to user

| Group | Type | Members | Purpose |
|-------|------|---------|---------|
| `Intune-Pilot-Devices` | Security, Assigned | Test device | Target the device regardless of who signs in, and test device-targeted assignments (for example an app installed per device rather than per user) |

## Verification

- [ ] Microsoft 365 E3 trial present, with seat count and end date recorded
- [ ] `pilot` exists with a usage location
- [ ] `Intune-Pilot` contains `pilot`
- [ ] `pilot` shows the E3 license assigned through the group
- [ ] MDM user scope set
- [ ] Break-glass account created and documented offline
- [ ] Admin account can open intune.microsoft.com and see Devices and Apps

## Screenshots

Add sanitized screenshots to `images/`:

- *`licenses.png`*<img width="600" height="229" alt="control prove" src="https://github.com/user-attachments/assets/fb040430-d7ba-4f2c-8a98-598603f92965" /> <img width="600" height="722" alt="assign license" src="https://github.com/user-attachments/assets/ec2c5e47-672d-45db-952e-df3be8a5997b" />

- *`mdm-scope.png`*<img width="1920" height="1080" alt="intune scope" src="https://github.com/user-attachments/assets/408cdf6b-984e-484e-ad40-da64715e4023" />



## Notes and issues

- *(Record anything unexpected.)*
