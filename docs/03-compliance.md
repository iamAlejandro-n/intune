# 03 - Compliance Policies

## Objective

Create a Windows compliance policy that evaluates device health, and confirm the test device reports Compliant or a clear failure reason.

## Prerequisites

- Device enrolled ([enrollment](02-enrollment.md))
- `Intune-Pilot` group created

## Policy design

| Setting | Value | Reason |
|---------|-------|--------|
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

- [✔] Policy shows as assigned
- [✔] Device status is Compliant, or Not compliant with a reason
- [✔] Policy JSON exported to `policies/compliance/`

## Export

```powershell
./scripts/export-policies.ps1 -Type compliance
```

Remove tenant IDs and any identifying values before committing.

## Screenshots

- `compliance-policy-settings.png`<img width="640" height="649" alt="compliance policy1" src="https://github.com/user-attachments/assets/e65ff0f3-9532-41bb-95be-8d718370ae2d" /><img width="895" height="391" alt="compliance policy2" src="https://github.com/user-attachments/assets/68cf464c-085f-4c67-9a42-fd7644d7b501" /><img width="792" height="369" alt="compliance policy3" src="https://github.com/user-attachments/assets/e9e2639c-8709-40c9-a6bf-90370ebf152f" />



- `compliance-device-status-good.png`<img width="670" height="415" alt="compliance applied" src="https://github.com/user-attachments/assets/f134fba0-306e-41d1-81e7-3f4c521dd35e" />
<img width="592" height="498" alt="compliance passing-b4-failing" src="https://github.com/user-attachments/assets/18d39142-338c-43a9-b4e1-6ae269327a08" />

- `compliance-device-status-failed.png`<img width="610" height="549" alt="compliance failed" src="https://github.com/user-attachments/assets/b7892c7b-89ef-4125-ad99-4a5577806ef9" />


## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Status stays "Not evaluated" | Device has not synced | Force sync, wait several minutes |
| A setting shows Not applicable | Setting not supported on Windows Home | Remove it, or record it as a Home-edition limitation |
| Everyone noncompliant | Mark-noncompliant action set to immediate | Add a grace period |

## Notes and issues

- i created a compliance policy and add rules
- add my pilot group and excluded breakglass
- check if added device complies with rules
- on my added device remove firewall to break a rule
- check that it show noncompliant


