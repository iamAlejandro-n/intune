# 04 - Configuration Profiles

## Objective

Push settings to the test device with the settings catalog and a Windows Update ring, and verify they apply.

## Prerequisites

- Device enrolled and compliant ([compliance](03-compliance.md))

## Profiles in this lab

| Profile | Type | Purpose |
|---------|------|---------|
| `Win-Lockscreen-Timeout` | Settings catalog | Low-risk first test |
| `Win-Edge-Baseline` | Settings catalog | Browser settings |
| `Win-Update-Ring` | Update ring | Windows Update behavior |

Start with a harmless setting (lock screen timeout, wallpaper) before anything restrictive.

## Steps

### Settings catalog profile

1. Intune admin center > **Devices > Configuration > Create > New policy**.
2. Platform: Windows 10 and later. Profile type: **Settings catalog**.
3. Name: `Win-Lockscreen-Timeout`.
4. Add the setting you want (for example, machine inactivity limit).
5. Assign to `Intune-Pilot`.
6. Sync the device.

### Update ring

1. **Devices > Windows updates > Update rings > Create profile**.
2. Set servicing channel, deferral periods and restart behavior.
3. Assign to `Intune-Pilot`.

## Verification

- [ ] Profile status: Succeeded in the Intune report
- [ ] Setting visible on the device (Settings, registry or `gpresult` equivalent)
- [ ] JSON exported to `policies/configuration/`

On the device, you can confirm applied MDM settings under Settings > Accounts > Access work or school > Info > **Create report** (MDM diagnostics).

## Screenshots

- `config-profile-settings.png`
- `config-profile-status.png`
- `setting-on-device.png`

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Profile stays Pending | Device has not checked in | Force sync |
| Conflict status | Two policies set the same value | Remove the duplicate |
| Not applicable | Wrong edition or OS version | Check platform and edition |

## Notes and issues

- *(Record anything unexpected.)*
