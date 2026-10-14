
# 02 - Enrollment

## Objective

Enroll a Windows 11 test device into Intune and confirm it appears as a managed device assigned to the `pilot` user.

## Prerequisites

- [Tenant setup](01-tenant-setup.md) completed
- A Windows 11 Pro, Enterprise or Education device (VM or spare PC). Windows Home cannot be Entra joined.
- Backup taken and BitLocker recovery key noted if using a real device

## Choose an enrollment method

| Method | Impact on device | Use when |
|--------|------------------|----------|
| Entra registered (Settings > Accounts > Access work or school > Connect) | Low, easy to remove | Personal or real device |
| Entra joined (during OOBE or Settings) | Full corporate management | VM or spare PC |
| Autopilot | Requires reset and device registration | Dedicated lab device only |

Method used in this lab: **`<fill in>`**

## Steps

1. Sign in to the device with a local account.
2. Open **Settings > Accounts > Access work or school**.
3. Select **Connect** (registered) or **Join this device to Microsoft Entra ID** (joined).
4. Sign in as `pilot@<tenant>.onmicrosoft.com`.
5. Complete MFA if prompted.
6. Open the Intune admin center and go to **Devices > All devices**.
7. Confirm the device appears, with the primary user set to `pilot`.
8. Force a sync: Settings > Accounts > Access work or school > Info > **Sync**.

## Verification

- [ ] Device visible in Intune admin center
- [ ] Management state shows as managed by Intune
- [ ] Primary user is `pilot`
- [ ] Last check-in time is recent

## Screenshots

- `enroll-connect.png`
- `device-in-intune.png`
- `device-overview.png`

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Device does not appear in Intune | MDM user scope not set | Check Entra > Mobility (MDM and MAM) |
| "Something went wrong" at sign-in | License missing | Assign Intune license to user |
| Device shows only in Entra, not Intune | Registered but not MDM-enrolled | Verify automatic enrollment scope |

## Cleanup

Use **Retire** in Intune to remove company data, then disconnect under Settings > Accounts > Access work or school. Use **Wipe** only on a VM.

## Notes and issues

- *(Record anything unexpected.)*
