# 05 - App Deployment

## Objective

Deploy one Microsoft Store app and one packaged Win32 app to the test device, with a working detection rule.

## Prerequisites

- Device enrolled
- For Win32: the Win32 Content Prep Tool (`IntuneWinAppUtil.exe`) from Microsoft, and an installer to package

## Apps in this lab

| App | Type | Install behavior |
|-----|------|------------------|
| `<Store app>` | Microsoft Store | Required |
| `<Win32 app>` | Win32 (.intunewin) | Required |

## Steps

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

- [ ] App installs without user interaction
- [ ] App status shows Installed in the Intune report
- [ ] Detection rule correctly reports installed and not installed

## Screenshots

- `app-store-assignment.png`
- `app-win32-detection.png`
- `app-install-status.png`

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Install loops or fails repeatedly | Detection rule never succeeds | Fix the detection rule |
| Install does nothing | Wrong silent switch | Test the command manually first |
| Slow to appear | Device not synced | Force sync, check the Intune Management Extension log |

## Notes and issues

- *(Record anything unexpected.)*
