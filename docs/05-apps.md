# 05 - App Deployment

## Objective

Deploy an app to the test device with Intune and verify the result. On this lab's Windows 11 Home device, the planned app types are not supported, so this page records the failure, the reason and the evidence.

## Prerequisites

- Device enrolled ([enrollment](02-enrollment.md)) and syncing
- `Intune-Pilot` group containing `pilot`
- For a Win32 app on a supported edition only: the Win32 Content Prep Tool (`IntuneWinAppUtil.exe`) from Microsoft, and an installer to package

## Result

**T4 failed: Microsoft Store app (new) deployment is not supported on Windows 11 Home.**

| Item | Value |
|------|-------|
| App | Company Portal (Microsoft Store app (new)) |
| Assignment | Required, `Intune-Pilot` |
| Install behavior | System (required for Entra registered devices) |
| Intune status | Agent installation failed, error code 0x0, Unknown |
| Device | Windows 11 Home, Entra registered |
| Cause | Windows Home does not support this app type |

### Why it failed

Microsoft's Windows app deployment documentation lists which app types each Windows edition supports:

- **Not supported on Home:** Microsoft Store app (new), Win32 (.intunewin), MSI, Office, Microsoft Edge.
- **Supported on Home:** LOB apps packaged as APPX or MSIX, Web Apps (web links) and Store Link apps.

The Intune console shows only a generic `0x0 Unknown` error, which does not name the edition. I identified the cause by checking the documentation after the deployment failed. An earlier assumption in this project, that Store apps should work on Home, was wrong.

### What I tried

- Assigned Company Portal as a Store app (new), Required, with System install behavior.
- Confirmed the device was enrolled, compliant and syncing.
- Checked the Intune Management Extension and Store service as possible causes before finding the edition limitation.

### Evidence

Primary evidence is the Microsoft documentation in the Sources section below. The screenshots below are supporting evidence only: they are AI-generated search summaries, not Microsoft documentation, and are included because they match what Microsoft's page states.

## Other findings

- On an Entra registered device, Store app (new) installs must use **System** context. A User-context app assigned as Available shows "Requirements Not Met" in the Company Portal (per Microsoft's Store apps documentation).
- Win32 app deployment is also not supported on Home, so the planned Win32 test was not run.

## Optional: test that does work on Home

A **Web app (Windows web link)** is listed as supported on Home.

1. Intune admin center > **Apps > Windows > Add**.
2. App type: **Web link**. Name it and enter a URL.
3. Assign **Required** to `Intune-Pilot`, then sync the device.
4. Verify that the shortcut appears on the device.

This creates a shortcut, not a real installation, so label it that way in the results.

## Steps on a supported edition (reference)

These steps apply to Pro, Enterprise or Education devices.

### Microsoft Store app

1. Intune admin center > **Apps > Windows > Add**.
2. App type: Microsoft Store app (new).
3. Search for the app and select it.
4. Assignment: **Required** for `Intune-Pilot`.

### Win32 app

1. Package the installer:
   ```powershell
   .\IntuneWinAppUtil.exe -c <source-folder> -s <setup-file> -o <output-folder>
   ```
2. Apps > Windows > Add > **Windows app (Win32)**, then upload the `.intunewin`.
3. Install command: `<silent install command>`
4. Uninstall command: `<silent uninstall command>`
5. Detection rule: file, registry key or MSI product code that proves it is installed.
6. Assign as **Required** to `Intune-Pilot`.

Keep install and detection scripts in `scripts/apps/`.

## Verification

- [x] Failure recorded with the exact error and the cause
- [✔] Intune status screenshot added (`app-install-status.png`)
- [x] Optional: web link shortcut appears on the device

## Screenshots
- `app-configuration.png` -<img width="867" height="661" alt="intune-app-config" src="https://github.com/user-attachments/assets/5701cac0-9d3b-43ae-acfb-550873d293aa" />

- `app-install-status.png` -<img width="594" height="654" alt="intune app status" src="https://github.com/user-attachments/assets/b917b3b6-688b-4594-8d9d-e21b115aa191" />

- `images/limitation_of_home.jpg` -<img width="1056" height="250" alt="limitation of home" src="https://github.com/user-attachments/assets/4f25d0c7-4c65-4bb3-a321-5eec23b76e00" />

- `images/home_limitations_2.jpg` -<img width="990" height="259" alt="home limitations 2" src="https://github.com/user-attachments/assets/44aeef6d-4165-48da-8d51-cd100835fb6d" />


## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Agent installation failed, 0x0, Unknown | App type not supported on this Windows edition | Check the edition support table in Microsoft's documentation |
| Install loops or fails repeatedly (supported edition) | Detection rule never succeeds | Fix the detection rule |
| Install does nothing (supported edition) | Wrong silent switch | Test the command manually first |
| Slow to appear | Device not synced | Force sync, check the Intune Management Extension log |
| Requirements Not Met in Company Portal | User-context Store app on an Entra registered device | Use System context |

## Sources

- [Deploy Windows apps with Intune (Microsoft Learn)](https://learn.microsoft.com/en-us/intune/app-management/deployment/deploy-windows)
- [Add Microsoft Store apps to Intune (Microsoft Learn)](https://learn.microsoft.com/mem/intune/apps/store-apps-microsoft)

## Notes and issues

- *(Record anything unexpected.)*
