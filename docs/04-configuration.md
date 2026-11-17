# 04 - Configuration Profiles

## Objective

Push a setting to the test device with the settings catalog and verify it applies, both in Intune and on the device itself.

## Prerequisites

- Device enrolled and compliant ([compliance](03-compliance.md))
- Windows Firewall back on if it was turned off for the compliance test

## Profiles in this lab

| Profile | Type | Purpose |
|---------|------|---------|
| `Win-Defender-Config` | Settings catalog | Main test: a Defender setting with a value that is easy to verify on the device |
| `Win-Edge-Baseline` | Settings catalog | Optional: verify at `edge://policy` |
| `Win-Lockscreen-Timeout` | Settings catalog | Optional experiment: may show Not applicable on Windows Home |
| `Win-Update-Ring` | Update ring | Optional: may be only partly supported on Windows Home |

The test device runs Windows 11 Home, so not every setting applies. Several personalization and lock screen policies are limited to Enterprise and Education editions. Record anything that shows Not applicable in the Home limitations table in [testing results](07-testing-results.md).

## Steps

### Defender settings catalog profile

1. Intune admin center > **Devices > Configuration > Create > New policy**.
2. Platform: **Windows 10 and later**. Profile type: **Settings catalog**.
3. Name: `Win-Defender-Config`.
4. **Add settings**, browse **Microsoft Defender Antivirus** (or search "signature"), and choose **Check for signature updates before running a scan**. Set it to **Enabled**. This setting normally defaults to off, so a change is visible.
5. Assign to `Intune-Pilot`.
6. Before the profile applies, record the starting value on the device (PowerShell, no admin rights needed):
   ```powershell
   Get-MpPreference | Select-Object CheckForSignaturesBeforeRunningScan
   ```
   Expected: `False`.
7. Sync the device: Settings > Accounts > Access work or school > Info > **Sync**. Wait a few minutes.
8. Run the same command again. Expected: `True`.

### Optional: other profiles

- **Edge:** add a Microsoft Edge setting such as "Show Home button on toolbar". Verify at `edge://policy` after clicking Reload policies.
- **Lock screen:** add a lock screen or inactivity setting. If it shows Not applicable on Home, record it as a limitation.
- **Update ring:** Devices > Windows updates > Update rings > Create profile. Set the channel, deferrals and restart behavior, and assign to `Intune-Pilot`. Record what applies on Home.

## Verification

- [ ] Profile status: Succeeded for the pilot device in the Intune report
- [ ] `Get-MpPreference` shows `False` before and `True` after
- [ ] Any Not applicable results recorded in the Home limitations table
- [ ] JSON exported to `policies/configuration/`

Optional: Settings > Accounts > Access work or school > Info > **Create report** produces an MDM diagnostics report.

## Screenshots

- `config-profile-settings.png` - <img width="1083" height="499" alt="config defender scan settings 1" src="https://github.com/user-attachments/assets/15eb22e2-da9c-4cbc-b4cb-99ad6dc085c8" />


- `config-profile-status.png` - <img width="805" height="410" alt="config succeeded" src="https://github.com/user-attachments/assets/9b8926e8-e05e-47c7-a738-dc7d860af725" />


- `setting-on-device-before.png` - <img width="1561" height="226" alt="scan-false" src="https://github.com/user-attachments/assets/410406c9-4b55-4776-b922-4ddb88a3da23" />


- `setting-on-device.png` - <img width="1779" height="284" alt="scan-true" src="https://github.com/user-attachments/assets/85ad6a95-0712-4528-9b14-85c71136908c" />



## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Profile stays Pending | Device has not checked in | Force sync |
| Conflict status | Two policies set the same value | Remove the duplicate |
| Not applicable | Setting not supported on Windows Home | Check edition support, record it as a limitation |
| `Get-MpPreference` not recognized | Command typed in Command Prompt instead of PowerShell, or Defender is not the active antivirus | Open Windows PowerShell and retry. If it persists, check Windows Security and `Get-Service WinDefend` |
| Value does not change after sync | Sync not finished, or the setting is blocked on this edition | Wait, sync again, check the per-setting status |

## Notes and issues

- *(Record anything unexpected.)*
