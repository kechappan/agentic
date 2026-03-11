# Android SDK 36 Upgrade Skill

### Description
Automates the process of upgrading the Android application to Android SDK 36.

### Usage
`/android-sdk-36-upgrade`

### Instructions
Act as an Expert Android Security & UI Engineer. Your internal knowledge cutoff may be prior to the final release of Android 16 (API level 36, "Baklava"). Use the strict rules, deprecations, and behavior changes listed in this skill to audit, refactor, and write all code for this Android banking app upgrade. Whenever code is provided, actively scan it against these API 36 rules. Do not suggest legacy fixes.

### Prerequisites
* Ensure target SDK and compile SDK are set to 36 in app-level `build.gradle`.
* Update to the latest compatible Android Gradle Plugin (AGP) and Kotlin versions.
* Identify all C/C++ native libraries (NDK) used in the project (e.g., custom cryptography or biometric wrappers).

### Upgrade Steps
1. **UI & Edge-to-Edge:** The `windowOptOutEdgeToEdgeEnforcement` flag is strictly deprecated. Ensure all screens implement `WindowInsetsCompat` to handle system bar padding so status/navigation bars do not obscure banking UI elements.
2. **Navigation:** Predictive back is now enabled by default. Migrate all `onBackPressed()` logic to `OnBackInvokedCallback` or `OnBackInvokedDispatcher` to prevent accidental transaction cancellations.
3. **Adaptive Layouts:** On screens >= 600dp, Android 16 ignores manifest restrictions (`screenOrientation`, `resizableActivity="false"`). Update layouts using `ConstraintLayout` or Compose adaptive guidelines so dashboards do not stretch or break.
4. **Background Tasks:** Update `JobScheduler` logic to maintain a strong reference to the `JobParameters` object. Handle the new `STOP_REASON_TIMEOUT_ABANDONED` to prevent system throttling.
5. **Security & Intents:** Ensure any Explicit Intent targeting an internal component strictly matches that component's `IntentFilter`. Vet all `PendingIntents` to prevent redirection attacks.
6. **Accessibility:** Replace deprecated `announceForAccessibility` with `setAccessibilityLiveRegion` (or Compose equivalent).
7. **NDK Alignment:** Recompile all native C/C++ libraries with `PAGE_SIZE=16384` to support 16 KB memory pages.

### Validation Steps
* Enable the "Edge-to-Edge" flag in Developer Options and verify no buttons or account balances are hidden behind system bars.
* Test critical banking flows (e.g., wire transfers) with the predictive back gesture to ensure state is maintained.
* Launch the app on a tablet emulator (>= 600dp) to verify layouts render correctly in forced multi-window or landscape modes.
* Trigger internal Intents (e.g., Auth to Transfer screen) to ensure the OS does not block them due to unmatched Intent Filters.
* Run the app to ensure no "Compatibility Mode" OS warnings appear regarding 4 KB memory pages.

### Known Issues
* `onBackPressed()` is no longer called, and `KeyEvent.KEYCODE_BACK` is no longer dispatched on API 36.
* Legacy background jobs that lose focus will be flagged as "Abandoned" and heavily throttled.
* Apps utilizing `announceForAccessibility` will experience broken screen reader flows.

### Rollback Plan
* Revert `targetSdk` and `compileSdk` to 35 in `build.gradle`.
* Restore legacy `onBackPressed()` handling for navigation routing.
* Revert to previous NDK build scripts if 16 KB alignment causes unresolvable crashes in 3rd-party security libraries.

### Files to Modify
* `build.gradle` (App level) / `libs.versions.toml`
* `AndroidManifest.xml` (Intent filters, permissions)
* `MainActivity.kt` and all routing Activities/Fragments
* Layout XML files / Compose UI files
* C/C++ NDK build scripts (e.g., `CMakeLists.txt`)

### Testing Requirements
* Android 16 (API 36) physical device or Emulator.
* Large screen device or emulator (smallest width >= 600dp).
* Screen reader / TalkBack enabled for accessibility verification.

### Notes
Flag any deprecated calls, missing Insets handling, non-adaptive layout constraints, unaligned Explicit Intents, or background job mismanagement immediately upon reviewing user-submitted code. Prioritize transaction security and UI stability.
