# Report: The Story of TrollStore and Breaking the iOS Trust Chain

## Key Terminology (Before Diving In)

* **CoreTrust:** A pivotal component within iOS related to the chain of trust and the verification of signatures and certificates. It essentially determines whether a given signature or certificate chain is considered trustworthy enough to allow an application to be installed and executed under system policies.

* **AMFI (Apple Mobile File Integrity):** A security layer that controls the acceptance or rejection of executing files and applications based on signature integrity and system policies. It acts as the "execution enforcer," applying Apple's rules regarding what is permitted to run.

* **Entitlements:** Signed privileges embedded within an application that dictate its ability to access sensitive interfaces or capabilities. Many of these are strictly granted only to system applications or highly privileged signed apps, as they provide a broader scope of access than standard applications.

* **Perma-signed (Permanent Sideloading):** A widely used community term indicating that an application remains installed and functional permanently without the need for periodic re-signing. The crucial aspect here is the practical outcome for the user: the continuous, uninterrupted operation of the app.

* **SPTM (Secure Page Table Monitor):** A modern, hybrid protection mechanism introduced in iOS 17. It is neither purely software nor purely hardware but a combination of both. Its primary strength lies in its hardware-backed nature, meaning that bypassing it typically requires interacting directly with the device's hardware.

* **Sideloading:** The process of installing iOS applications from outside the official App Store via various methods (such as IPA files). This is usually limited by signing constraints or expiration periods depending on the method utilized.

* **CVE (Common Vulnerabilities and Exposures):** An official identifier for security vulnerabilities used to document, track, and link flaws to their respective patches across different software versions.

## 1.0 - The Beginning

This report covers the story of TrollStore.

How did it manage to achieve permanent installation of IPA applications without a full jailbreak? How did a vulnerability surface within CoreTrust across two different phases (TrollStore 1 followed by TrollStore 2) yielding similar final results? And what are the technical differences between them?

In simple terms: What exactly broke within the chain of trust? And how did the community recycle the same core concept when a new flaw emerged in the exact same area?

## 1.1 - What is TrollStore?

**Project: TrollStore** is a mechanism that allows the permanent installation and execution of IPA files (perma-signed from the user's perspective). 
To be technically precise: it does not entirely compromise the system like a traditional jailbreak. Instead, it exploits a logic flaw in the signature-related trust chain, forcing iOS to accept a signature in an unexpected manner. Consequently, it gains the ability to install applications permanently with elevated privileges (dictated by entitlements).

### 1.1.1 - Why Do Some Prefer It Over a Jailbreak?

It provided the perfect middle ground for many users:
* Permanent installation of applications and tools.
* Avoidance of opening all the security floodgates and risks associated with a full jailbreak.
* Provision of a significant scope for powerful tools (relying on high-level permissions and entitlements).

## 1.2 - Two Versions, One Core Concept

The community generally divides it as follows (more as a chronological timeline than two entirely separate projects):

* **Project: TrollStore 1**
  * Scope: Approximately iOS 14.0 → iOS 15.4.1 (and potentially some iOS 15.5 betas).
  * Relied on vulnerability: `CVE-2022-26766` (Linus Henze).

* **Project: TrollStore 2**
  * Revived the same concept and result on newer firmware via vulnerability: `CVE-2023-41991`.
  * Supported a broad range spanning: iOS 14.0 beta 2 → 16.6.1 + 16.7 RC + 17.0.

> **Crucial Note:**
> Having base support for a specific device/firmware combination is one thing, but possessing a suitable installation method is another. The installation process requires an entry point (exploit) that varies depending on the iOS version and system environment.

## 1.3 - Who Worked on It? And Who Discovered the Flaws?

* **Lead Developer:** opa334
* **Key Discoverers:**
  * Vulnerability: `CVE-2022-26766` - Linus Henze
  * Vulnerability: `CVE-2023-41991` - Citizen Lab + Google TAG

## 1.4 - How Did It Occur in CoreTrust Twice?

The answer is that the exact same technical details did not repeat; rather, the vulnerability occurred in the same component (CoreTrust), but involved a different type of logical flaw. 
To be exact: The end result repeated (bypassing signature rules), but it was achieved through entirely different vulnerabilities within the same component.

### 1.4.1 - TrollStore 1 (High Trust / Chain Logic)
Vulnerability `CVE-2022-26766` involved a critical flaw: the ability to pass a trust state or a certificate chain in a way that artificially elevated the system's acceptance level of the signature. The application appeared to belong to a highly trusted category, thereby unexpectedly obtaining a wide array of entitlements.

### 1.4.2 - TrollStore 2 (Multiple Signers Logic)
Vulnerability `CVE-2023-41991` was fundamentally different: a logic error in the validation mechanism when files were signed by more than one signer. This resulted in the incorrect acceptance of a signature, ultimately reproducing the identical final outcome: bypassing the signature verification rules within CoreTrust.

### 1.4.3 - Why Did Apple Take CVE-2023-41991 So Seriously?
Because `CVE-2023-41991` was actively linked to real-world exploit chains (meaning it was practically utilized by spyware organizations). The involvement of entities like Google TAG and Citizen Lab escalated the severity of the issue, as their reports typically emerge when an exploit is active in the wild, rather than being a mere theoretical discovery by an independent security researcher.

## 1.5 - Simplifying the Concept: CoreTrust + AMFI

To break it down:
* **AMFI** is responsible for file integrity and signature enforcement on iOS.
* **CoreTrust** assists in determining whether the signature or trust chain is deemed acceptable and trustworthy by the system.

The core idea: If you can deceive the system into incorrectly believing your application is highly trusted, the doors to restricted entitlements suddenly open. 
This is where TrollStore stepped in: it weaponized a trust/validation flaw, making permanent sideloading possible without a traditional jailbreak.

## 1.6 - How Does It Differ From a Jailbreak?

* **Jailbreak:** Goes all the way (modifying the root file system, breaking most security mitigations, and allowing tweaks with maximum privileges).
* **Project: TrollStore:** Grants you permanent installation and entitlement expansion, but does not automatically transition your device into a fully compromised, jailbroken state.

In short: It is a "Super Sideload" built upon a trust logic flaw, not a total system compromise.

## 1.7 - TrollStore Installation Methods

The concept: TrollStore relies on a flaw inside CoreTrust, but the practical hurdle was always: How do we initially install it? 
This gave rise to a landscape of community tools, where each tool leveraged a different entry point depending on the device and firmware, ultimately achieving the same objective: injecting TrollStore, installing it, and running it stably.

* **Tool: TrollInstallerX**
  * **Role:** A universal installer aiming to cover the widest possible firmware ranges, eliminating the need for fragmented, version-specific tools.
  * **Reliance:** Relies on finding an appropriate entry point within the system/firmware to pass the installation execution (the entry point varies by version and device architecture). It then utilizes this entry to finalize the TrollStore installation.
  * **Significance:** It reduces community fragmentation. Instead of navigating 3–4 different solutions, it provides a unified option covering a vast majority of scenarios.

* **Tool: TrollInstallerMDC**
  * **Role:** An installer tied to a specific era dominated by solutions built on the MacDirtyCow exploit.
  * **Reliance:** Relies on the MacDirtyCow exploit (exploiting a system behavior allowing limited system-level modifications), acting as a bridge to prepare and execute the TrollStore installation.
  * **Significance:** It was the optimal entry point during its time for specific iOS ranges, particularly when alternative installation channels were scarce.

* **Tool: TrollHelper**
  * **Role:** Acted more as a utility assistant than a comprehensive installer, functioning as an intermediary solution based on the available channel in that specific firmware.
  * **Reliance:** Relies on a specific context or entry point available in a given environment that permits the injection of TrollStore or the activation of its persistence component.
  * **Significance:** It was a popular and straightforward option during specific periods, especially when entry points were limited or when users preferred lightweight solutions.

## 1.8 - The TrollRestore Concept (The Genius of the Restore Context)

**Concept: TrollRestore** capitalized on the backup/restore path as a legitimate system context where the enforcement of certain restrictions behaves differently. This opened the door to installing TrollStore on firmware ranges where the primary challenge was "how to gain entry," rather than "does the CoreTrust flaw exist?" (This specifically addresses iOS 17.0, where the CoreTrust flaw existed, but conventional entry methods like KFD and TrollInstallerX were entirely patched).

The general idea: In iOS, not all processes handle files and applications with the exact same level of scrutiny.
During standard daily usage, restrictions are draconian because the system assumes any new binary must pass rigorous validation paths (signatures, trust, entitlements).
However, during the Restore phase, the system enters a distinct operational mode: its primary objective is to reconstruct an old device state as accurately as possible.

The brilliant pivot: The restore path processes data bundles (app data, settings, file structures) and reconstructs them on the device. This path frequently operates via services possessing higher privileges than standard applications, performing reconstruction operations rather than traditional installations.

Where was the vulnerability/opportunity in TrollRestore?
The opportunity lay at the intersection of two elements:
1. The CoreTrust flaw (which allows the final outcome: incorrect acceptance of a signature/trust state).
2. The Restore context (which provides an alternative entry channel, as the system rebuilds data within a service environment distinct from the daily installation path).

In clearer terms: 
Instead of forcing entry through the highly guarded standard installation door, it utilized the data reconstruction door—which occasionally handles parameters differently. When coupled with the presence of the CoreTrust flaw, this door proved sufficient to reach the TrollStore installation milestone on firmwares where gaining initial entry was the primary roadblock.

## 1.9 — Vulnerabilities and the End of TrollStore?

* **Vulnerability 1: CVE-2022-26766** — CoreTrust Flaw (Certificate Parsing)
  * Patched in: iOS 15.5
* **Vulnerability 2: CVE-2023-41991** — CoreTrust Flaw (Certificate Validation/Signature Bypass)
  * Patched in: iOS 16.7 and iOS 17.0.1
* **Vulnerability 3: CVE-2022-46689** — MacDirtyCow (Copy-On-Write flaw in the XNU kernel)
  * Patched in: iOS 15.7.2 and iOS 16.2
* **Vulnerability 4: CVE-2024-44252** — Restore/Backup Path Flaw (MobileBackup)
  * Patched in: iOS 17.7.1 and iOS 18.1

### 1.9.1 - The Post-TrollStore Era

Following the TrollStore era, Apple sealed the CoreTrust vulnerabilities actively exploited by the project. Furthermore, with the introduction of hardware-backed mitigation layers such as SPTM (Secure Page Table Monitor) and TXM (Trusted Execution Monitor) in iOS 17 for A15 devices and newer, possessing kernel read/write capabilities no longer guarantees the ability to modify page tables, alter memory permissions, or seize full control over the signature validation logic as it did in older generations.

The SPTM system acts as the ultimate authority for protecting and managing page tables and memory retyping operations, while TXM is isolated as the dedicated component responsible for verifying signatures and entitlements—residing within trust boundaries outside the direct control of the XNU kernel.

Consequently: Even if a future vulnerability emerges within CoreTrust resembling its predecessors (in terms of bypassing signature validation), achieving the historical impact of TrollStore (permanent installation with broad privileges without a traditional jailbreak) will generally be significantly harder. Achieving full impact may necessitate bypassing layers above the kernel (such as SPTM/TXM), rather than merely exploiting kernel + root. Nevertheless, it is impossible to declare that a future CoreTrust flaw will be entirely useless, but it is certain that the current security environment makes replicating the TrollStore scenario incredibly difficult.

## 2.0 - Conclusion

Project: TrollStore was (and remains) one of the most brilliant engineering achievements of the jailbreak community because it established a point of equilibrium and a genius concept:
- User freedom
- Permanent installation
- Mitigated risk compared to a full system compromise

On the flip side, its story serves as a critical reminder: 
Even seemingly impenetrable systems enforcing robust protections like CoreTrust and AMFI can harbor a minor verification oversight. That single oversight has the power to completely alter the rules of the game. The story does not conclude here; it remains a continuous game of cat and mouse as long as technology continues to evolve.
