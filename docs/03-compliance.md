# 03 - Compliance Policies

## Objective

Create a Windows compliance policy that evaluates device health, and confirm the test device reports Compliant or a clear failure reason.

## Prerequisites

- Device enrolled ([enrollment](02-enrollment.md))
- `Intune-Pilot` group created

## Policy design

| Setting | Value | Reason |
|---------|-------|--------|
| Require BitLocker | Yes | Protects data at rest |
| Minimum OS version | `<e.g. 10.0.22631>` | Ensures patched OS |
| Microsoft Defender real-time protection | Required | Baseline malware protection |
| Firewall | Required | Network baseline |
| Mark noncompliant after | `<e.g. 1 day>` | Grace period for fixes |

Start lenient: only require what the device already meets, then tighten.

## Steps

1. Intune admin center > **Devices > Compliance > Create policy**.
2. Platform: **Windows 10 and later**.
3. Name: `Win-Compliance-Baseline`.
4. Configure the settings above.
5. Set the actions for noncompliance (mark noncompliant, send notification).
6. Assign to `Intune-Pilot`.
7. Sync the device and wait for evaluation.

## Verification

- [ ] Policy shows as assigned
- [ ] Device status is Compliant, or Not compliant with a reason
- [ ] Policy JSON exported to `policies/compliance/`

## Export

```powershell
./scripts/export-policies.ps1 -Type compliance
```

Remove tenant IDs and any identifying values before committing.

## Screenshots

- `compliance-policy-settings.png`
- `compliance-device-status.png`

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Status stays "Not evaluated" | Device has not synced | Force sync, wait several minutes |
| BitLocker shows noncompliant | Drive not encrypted | Enable BitLocker or a policy that does |
| Everyone noncompliant | Mark-noncompliant action set to immediate | Add a grace period |

## Notes and issues

- *(Record anything unexpected.)*
