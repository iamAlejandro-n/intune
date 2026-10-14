# Intune Lab

A hands-on Microsoft Intune and Entra ID lab. It covers device enrollment, compliance, configuration, app deployment and Conditional Access

> **Note:** This is a personal lab built in a test tenant. All identifiers (tenant IDs, domains, emails, serial numbers) have been removed or replaced with placeholders.

## What this project demonstrates

- Enrolling a Windows device into Intune
- Compliance policies (BitLocker, OS version, Defender) *update*
- Configuration profiles using the settings catalog
- App deployment (Microsoft Store)
- Conditional Access that requires a compliant device
- Exporting and importing policies with PowerShell and Microsoft Graph *maybe delete if not*
- Documented test cases with results

## Lab environment

| Item | Value |
|------|-------|
| Tenant | Test tenant (`mini.onmicrosoft.com`) |
| Licenses | Microsoft 365 E3 (Intune Plan 1 + Entra ID P1) |
| Test device | Windows 11 `physical device` |
| Test users | `pilot` and `control` |
| Test group | `Intune-Pilot` |
| Admin tooling | Intune admin center, Entra admin center | *include powershell if used*

## Architecture

```
 Test user ──signs in──▶ Entra ID ──Conditional Access──▶ Microsoft 365 apps
                              │                               ▲
                              │ device compliance state        │ allow / block
                              ▼                               │
 Windows device ◀──policies/apps── Intune ───────────────────┘
```



## Repository layout

```
docs/        Step-by-step write-ups (start at 01)
policies/    Exported, sanitized policy JSON
scripts/     PowerShell for export/import and app packaging
.github/     Workflow for JSON linting and secret scanning
```

## Documentation

1. [Tenant setup](docs/01-tenant-setup.md)
2. [Enrollment](docs/02-enrollment.md)
3. [Compliance policies](docs/03-compliance.md)
4. [Configuration profiles](docs/04-configuration.md)
5. [App deployment](docs/05-apps.md)
6. [Conditional Access](docs/06-conditional-access.md)
7. [Testing results](docs/07-testing-results.md)

## Results at a glance

| Test | Result |
|------|--------|
| Device enrollment | ✅ / ❌ |
| Compliance evaluation | ✅ / ❌ |
| Configuration profile applied | ✅ / ❌ |
| App installed silently | ✅ / ❌ |
| Conditional Access: compliant device allowed | ✅ / ❌ |
| Conditional Access: non-compliant device blocked | ✅ / ❌ |

See [testing results](docs/07-testing-results.md) for details and screenshots.

## How to reproduce

1. Follow [tenant setup](docs/01-tenant-setup.md).
2. Enroll a test device ([enrollment](docs/02-enrollment.md)).
3. Create the policies manually, or import them with `scripts/import-policies.ps1` after reviewing the JSON.
4. Run the test cases and record your results.

## Lessons learned

- *(Add 3-5 bullets after you finish: what broke, what surprised you, what you would do differently.)*

## Security notes

- No secrets, certificates or tokens are stored in this repo.
- Screenshots and JSON are sanitized.
- A break-glass admin account is excluded from all Conditional Access policies.

## License

MIT (see `LICENSE`)
