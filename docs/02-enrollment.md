# 02 - Enrollment

## Objective

Enroll a Windows 11 test device into Intune and confirm it appears as a managed device assigned to the `pilot` user.

## Prerequisites

- [Tenant setup](01-tenant-setup.md) completed
- A Windows 11 Pro, Enterprise or Education device. Windows Home cannot be Entra joined.This lab uses Windows 11 Home, which Intune supports for enrollment but which cannot be joined to Microsoft Entra ID.
- Backup taken and BitLocker recovery key noted if using a real device

## Choose an enrollment method

| Method | Impact on device | Use when | works on home |
|--------|------------------|----------|----------|
| Entra registered (Settings > Accounts > Access work or school > Connect) | Low, easy to remove | Personal or real device | yes |
| Entra joined (during OOBE or Settings) | Full corporate management | VM or spare PC |no requires pro, Enterprise or Education |
| Autopilot | Requires reset and device registration | Dedicated lab device only | no |

Method used in this lab: **`Entra registered with Connect, on Windows 11 Home.`**

Home edition notes
Windows sign-in stays on the local or personal Microsoft account. pilot is used for Connect, the browser and Microsoft 365 apps.
Not every Intune setting applies to Home. Check each setting's status in the Intune report: Succeeded, or Not applicable.
BitLocker is skipped in this lab. See compliance policies.

## Steps

1. Sign in to the device with a local account.
2. Open **Settings > Accounts > Access work or school**.
3. Select **Connect.** (The Join option is not available on Home.)
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

- `enroll-connect.png`<img width="738" height="400" alt="account login intune" src="https://github.com/user-attachments/assets/d87db925-a657-4e73-a4b0-d4ae909c3232" />

- `device-in-intune.png`
- `device-overview.png`<img width="1441" height="103" alt="intune device join" src="https://github.com/user-attachments/assets/40ab0185-9eae-4287-bb14-4e132b327c0d" />


## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Device does not appear in Intune | MDM user scope not set | Check Entra > Mobility (MDM and MAM) |
| "Something went wrong" at sign-in | License missing | Assign Intune license to user |
| Device shows only in Entra, not Intune | Registered but not MDM-enrolled | Verify automatic enrollment scope |
| No management prompt appears |"Disable MDM enrollment when adding work or school account" is on| leave setting off |
| Join option missing | Windows home | use connect |

## Cleanup

Use **Retire** in Intune to remove company data, then disconnect under Settings > Accounts > Access work or school. Use **Wipe** only on a VM.

## Notes and issues

- *(Record anything unexpected.)*
