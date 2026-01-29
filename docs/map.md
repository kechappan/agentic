# 🗺️ Project Caps: System & Architecture Map

## 1. High-Level Architecture
- **Build System:** [e.g., Gradle KTS / Groovy]
- **Core Frameworks:** [e.g., Dagger-Hilt, Jetpack Compose/XML]
- **Hybrid Bridge:** **Caps** (Internal Library)

---

## 2. The Caps Bridge (JS-to-Native Surface)
*Last updated by @Caps-Architect on [Date]*

| JS Interface Method | Native Destination / Class | Module | Purpose |
| :--- | :--- | :--- | :--- |
| `Caps.startPayment()` | `PaymentAuthActivity` | `:feature-pay` | Initiates 3DS authentication flow. |
| `Caps.getDeviceID()` | `DeviceUtil.kt` | `:core-utils` | Returns hardware UUID to web layer. |

---

## 3. Navigation & Entry Points
*Last updated by @Navigator on [Date]*

- **Main Launcher:** `com.bank.app.main.LauncherActivity`
- **Primary Nav Graph:** `res/navigation/main_nav.xml`
- **Deeplink Schemes:** `bankapp://`, `https://bank.com/link/`
- **Notification Handler:** `com.bank.app.push.NotificationService`

---

## 4. 3rd-Party SDK Inventory
*Last updated by @SDK-Analyst on [Date]*

| SDK Name | Version | Usage | Status |
| :--- | :--- | :--- | :--- |
| **Jumio** | 3.9.2 | Identity Verification | **Active** (Hybrid via Caps) |
| **Plaid** | 4.1.0 | Account Aggregation | **Legacy** (Native flow only) |

---

## 5. Tech Debt & Deletion Candidates (The Ghost List)
*Last updated by @Ghost-Hunter on [Date]*

- [ ] **Orphaned Activity:** `OldSettingsActivity.kt` (No longer in Manifest).
- [ ] **Zombie Dependency:** `com.unused:pdf-viewer` (Declared but zero imports).
- [ ] **Redundant Resource:** `layout/activity_legacy_login.xml` (Replaced by Caps flow).
