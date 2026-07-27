# Store privacy declarations — Adhkari

**Application:** Adhkari · `com.benbakhta.adhkari` · version 1.0.0
**Prepared:** `[date of submission]`

This document contains the official answers to be entered in Google Play Console and Apple App Store Connect, and the content of the iOS privacy manifest. Every answer below is consistent with `PRIVACY-POLICY.md` and with the application source code.

**Underlying facts (single source for every answer):** the app has no accounts, no login, no ads, no analytics SDK and no advertising identifiers. All user content is stored in a local SQLite database and never transmitted. The only outbound transmission is a crash diagnostic report sent to Sentry, and only while the in-app crash-reporting setting is enabled; the setting is off by default and is presented for an explicit decision on first launch. Performance/trace sampling is disabled.

---

## A) Google Play — Data safety

### A.1 Data collection and security practices

| Question                                                              | Answer                                                                                                                         |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Does your app collect or share any of the required user data types?   | **Yes**                                                                                                                        |
| Is all of the user data collected by your app encrypted in transit?   | **Yes** — HTTPS/TLS                                                                                                            |
| Do you provide a way for users to request that their data is deleted? | **Yes** — by email to hassantcm@gmail.com; the app also provides in-app deletion of all local data (Settings → Reset all data) |

### A.2 Data types — declare these three only

| Category → Data type                          | Collected | Shared | Processed ephemerally | Required or optional           | Purposes              |
| --------------------------------------------- | --------- | ------ | --------------------- | ------------------------------ | --------------------- |
| App info and performance → **Crash logs**     | **Yes**   | **No** | **No**                | **Optional** (user can choose) | **App functionality** |
| App info and performance → **Diagnostics**    | **Yes**   | **No** | **No**                | **Optional** (user can choose) | **App functionality** |
| Device or other IDs → **Device or other IDs** | **Yes**   | **No** | **No**                | **Optional** (user can choose) | **App functionality** |

**Diagnostics** covers only the session records (session start and whether the session ended in a crash) and the technical breadcrumbs attached to a crash report, both produced by `enableAutoSessionTracking` and both gated on the same in-app setting. **Other app performance data** is not collected: trace sampling is set to zero.

Do **not** select any other purpose (no Analytics, Advertising or marketing, Personalisation, Fraud prevention, Account management, Developer communications).

### A.3 Data types to declare as NOT collected

Location (approximate and precise) · Personal info (name, email address, user IDs, address, phone number, race and ethnicity, political or religious beliefs, sexual orientation, other) · Financial info · Health and fitness · Messages · Photos and videos · Audio files · Files and docs · Calendar · Contacts · App activity (interactions, in-app search history, installed apps, other user-generated content, other actions) · Web browsing history · App info and performance → **Other app performance data** · Device or other IDs beyond the single installation identifier above.

The backup export/import feature is **not** declared: backup files are written on the device and shared only to a destination the user chooses; the developer neither receives nor accesses them, so no collection or sharing occurs within Play's definitions.

### A.4 Sharing question

**Is any of this data shared with third parties? → No.**
Sentry is engaged as a **processor / service provider**, acting only on documented instructions and not using the data for its own purposes. Under the Play Data safety definitions this is **collection**, not **sharing**.

### A.5 Other Play Console declarations

| Question                                                                                                 | Answer                                                                                                                           |
| -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Privacy policy URL                                                                                       | `[public URL where PRIVACY-POLICY.md is hosted]` — required before submission                                                    |
| Does your app contain ads?                                                                               | **No** — no advertising SDK is present                                                                                           |
| App access — is any part of the app restricted by login?                                                 | **No** login required; provide no credentials                                                                                    |
| Account deletion (Data deletion section)                                                                 | The app **does not offer account creation**; select the corresponding option. Data deletion request channel: hassantcm@gmail.com |
| Government app                                                                                           | **No**                                                                                                                           |
| Financial features                                                                                       | **None**                                                                                                                         |
| Target audience and content                                                                              | **Not** targeted to children; the app is designed for a general audience of teens and adults                                     |
| Advertising ID permission (`com.google.android.gms.permission.AD_ID`)                                    | **Not declared** — the app does not use the advertising ID                                                                       |
| Permissions requiring a declaration form (all files access, SMS, call log, location in background, etc.) | **None requested**                                                                                                               |

---

## B) Apple — App Privacy (App Store Connect)

**Do you or your third-party partners collect data from this app? → Yes.**

### B.1 Data types — declare these three only

| Category → Data type                    | Linked to the user's identity | Used for tracking | Purpose               |
| --------------------------------------- | ----------------------------- | ----------------- | --------------------- |
| Diagnostics → **Crash Data**            | **Not Linked to You**         | **No**            | **App Functionality** |
| Diagnostics → **Other Diagnostic Data** | **Not Linked to You**         | **No**            | **App Functionality** |
| Identifiers → **Device ID**             | **Not Linked to You**         | **No**            | **App Functionality** |

**Other Diagnostic Data** covers the session records and breadcrumbs described in A.2, and matches `NSPrivacyCollectedDataTypeOtherDiagnosticData` in the app's privacy manifest (section C).

### B.2 Data types to declare as NOT collected

Contact Info · Health & Fitness · Financial Info · Location · Sensitive Info · Contacts · User Content (all sub-types) · Browsing History · Search History · Identifiers → User ID · Purchases · Usage Data (Product Interaction, Advertising Data, Other Usage Data) · Diagnostics → **Performance Data** · Surroundings · Body · Other Data.

**Performance Data must not be declared:** trace sampling is set to zero, so no performance measurements are collected.

### B.3 Related App Store Connect answers

| Question                                             | Answer                                                                                                                                                                                         |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Does this app use the Advertising Identifier (IDFA)? | **No**                                                                                                                                                                                         |
| App Tracking Transparency                            | **Not applicable** — no tracking, no `NSUserTrackingUsageDescription`                                                                                                                          |
| Account creation / sign-in                           | **None** — the account-deletion requirement (Guideline 5.1.1(v)) does not apply                                                                                                                |
| Third-party partners collecting data                 | **None**. Sentry is a service provider processing crash diagnostics on our behalf                                                                                                              |
| Privacy policy URL                                   | `[public URL where PRIVACY-POLICY.md is hosted]`                                                                                                                                               |
| Export compliance — does your app use encryption?    | The app uses only HTTPS/TLS provided by the operating system and standard platform keychain storage; it implements no proprietary encryption. Set `ITSAppUsesNonExemptEncryption` to **false** |

---

## C) iOS privacy manifest — `PrivacyInfo.xcprivacy`

The manifest is generated from the app config: `expo.ios.privacyManifests` in `app.json` is written to `ios/<project>/PrivacyInfo.xcprivacy` by `IOSConfig.PrivacyInfo.withPrivacyInfo`, which is part of the default iOS plugin set in `@expo/prebuild-config` (SDK 54). The declarations below are the exact content of that key and therefore of the generated file.

Because Apple does not reliably parse the privacy manifests bundled with statically linked CocoaPods, the required-reason categories used by the linked dependencies are repeated at app level, as Expo's Privacy manifests guide instructs. Each entry is evidenced by the manifest shipped inside the corresponding published package: `react-native@0.81.5` (`React/Resources`, `ReactCommon/cxxreact`), `expo-constants@18.0.13`, `expo-notifications@0.32.17`, `expo-system-ui@6.0.9`, `expo-file-system@19.0.23`, and `sentry-cocoa@8.57.0` (the `Sentry/HybridSDK` dependency of `RNSentry.podspec`).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>NSPrivacyTracking</key>
  <false/>

  <key>NSPrivacyTrackingDomains</key>
  <array/>

  <key>NSPrivacyCollectedDataTypes</key>
  <array>
    <dict>
      <key>NSPrivacyCollectedDataType</key>
      <string>NSPrivacyCollectedDataTypeCrashData</string>
      <key>NSPrivacyCollectedDataTypeLinked</key>
      <false/>
      <key>NSPrivacyCollectedDataTypeTracking</key>
      <false/>
      <key>NSPrivacyCollectedDataTypePurposes</key>
      <array>
        <string>NSPrivacyCollectedDataTypePurposeAppFunctionality</string>
      </array>
    </dict>
    <dict>
      <key>NSPrivacyCollectedDataType</key>
      <string>NSPrivacyCollectedDataTypeOtherDiagnosticData</string>
      <key>NSPrivacyCollectedDataTypeLinked</key>
      <false/>
      <key>NSPrivacyCollectedDataTypeTracking</key>
      <false/>
      <key>NSPrivacyCollectedDataTypePurposes</key>
      <array>
        <string>NSPrivacyCollectedDataTypePurposeAppFunctionality</string>
      </array>
    </dict>
    <dict>
      <key>NSPrivacyCollectedDataType</key>
      <string>NSPrivacyCollectedDataTypeDeviceID</string>
      <key>NSPrivacyCollectedDataTypeLinked</key>
      <false/>
      <key>NSPrivacyCollectedDataTypeTracking</key>
      <false/>
      <key>NSPrivacyCollectedDataTypePurposes</key>
      <array>
        <string>NSPrivacyCollectedDataTypePurposeAppFunctionality</string>
      </array>
    </dict>
  </array>

  <key>NSPrivacyAccessedAPITypes</key>
  <array>
    <dict>
      <key>NSPrivacyAccessedAPIType</key>
      <string>NSPrivacyAccessedAPICategoryUserDefaults</string>
      <key>NSPrivacyAccessedAPITypeReasons</key>
      <array>
        <string>CA92.1</string>
      </array>
    </dict>
    <dict>
      <key>NSPrivacyAccessedAPIType</key>
      <string>NSPrivacyAccessedAPICategoryFileTimestamp</string>
      <key>NSPrivacyAccessedAPITypeReasons</key>
      <array>
        <string>C617.1</string>
      </array>
    </dict>
    <dict>
      <key>NSPrivacyAccessedAPIType</key>
      <string>NSPrivacyAccessedAPICategoryDiskSpace</string>
      <key>NSPrivacyAccessedAPITypeReasons</key>
      <array>
        <string>E174.1</string>
      </array>
    </dict>
    <dict>
      <key>NSPrivacyAccessedAPIType</key>
      <string>NSPrivacyAccessedAPICategorySystemBootTime</string>
      <key>NSPrivacyAccessedAPITypeReasons</key>
      <array>
        <string>35F9.1</string>
      </array>
    </dict>
  </array>
</dict>
</plist>
```

**Justification of each API entry:**

- `NSPrivacyAccessedAPICategoryUserDefaults` → `CA92.1` — user defaults are read and written only to hold this app's own settings, accessible to no other app.
- `NSPrivacyAccessedAPICategoryFileTimestamp` → `C617.1` — file metadata is read only for files inside the app's own container, when writing and verifying an exported backup file and by the runtime's own caches.
- `NSPrivacyAccessedAPICategoryDiskSpace` → `E174.1` — available space is checked before writing files, so that a write failure can be handled rather than crashing.
- `NSPrivacyAccessedAPICategorySystemBootTime` → `35F9.1` — boot time is used only to measure elapsed time between events inside the app (session duration); the raw value is not sent off-device.

**Deliberately not declared:** `85F4.1` (disk space is never displayed to the user) · `0A2A.1` (a reason reserved for third-party SDKs, not for an app target) · `3B52.1` (no timestamp is read for user-selected files; import reads content only) · `NSPrivacyAccessedAPICategoryActiveKeyboards` (never accessed) · `NSPrivacyCollectedDataTypePerformanceData` (trace sampling is zero, so no performance data is produced despite Sentry's own SDK manifest listing it).

**Configuration note:** keep `ios/` and `android/` out of version control. If a committed `ios/` directory is present at build time, EAS treats the project as bare, prebuild does not run, and changes to `app.json` no longer reach the generated manifest.

---

## D) Evidence — where each answer comes from in the code

| Answer                                              | Evidence                                                                                                                                                                                                                                                                              |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Collection is optional and off by default           | `src/monitoring/sentrySetup.ts` — `loadSetting('crashReportingEnabled', false)`, `enabled: !__DEV__ && crashReportingEnabled`, `beforeSend` returns `null` when disabled; `src/store/dhikrStore.ts` — `setCrashConsent`; `src/screens/CrashConsentScreen.tsx` — first-launch decision |
| Crash logs / Crash Data                             | Sentry SDK with `attachStacktrace: true`, `enableNativeCrashHandling: true`, `enableAutoSessionTracking` gated on consent; `src/components/AppErrorBoundary.tsx` — `captureException`                                                                                                 |
| Device or other IDs / Device ID                     | `src/monitoring/installationId.ts` — random UUID in `expo-secure-store`; `src/app/App.tsx` — `Sentry.setUser({ id })` when consent is on, `Sentry.setUser(null)` when off                                                                                                             |
| No performance or behavioural data                  | `tracesSampleRate: 0` in `src/monitoring/sentrySetup.ts`                                                                                                                                                                                                                              |
| Encrypted in transit                                | Sentry DSN is an `https://` ingest endpoint                                                                                                                                                                                                                                           |
| No sharing, no sale, no ads                         | No advertising, analytics, attribution or billing dependency in `package.json`; the only outbound endpoint in the codebase is the Sentry DSN                                                                                                                                          |
| Diagnostics / Other Diagnostic Data                 | `enableAutoSessionTracking: crashReportingEnabled` in `src/monitoring/sentrySetup.ts` (session records), and the Sentry SDK's default breadcrumbs attached to a crash event                                                                                                           |
| No location, contacts, photos, microphone, calendar | No corresponding permission and no corresponding module; the audio library is configured with `microphonePermission: false` and `recordAudioAndroid: false`, so neither `RECORD_AUDIO` nor `NSMicrophoneUsageDescription` is produced                                                 |
| Permissions                                         | Resolved config (`npx expo config --type public`) → `android.permissions`: `RECEIVE_BOOT_COMPLETED`, `POST_NOTIFICATIONS`, `MODIFY_AUDIO_SETTINGS` (normal permission added by the audio playback library; no prompt, no Data safety data type)                                       |
| Local notifications only, no push tokens            | `src/services/notifications/notificationHelper.ts` — `scheduleNotificationAsync` with local daily triggers; no push-token API is called anywhere                                                                                                                                      |
| User content never transmitted                      | `src/database/db.ts` — local SQLite `dhikr.db`; export/import in `src/screens/SettingsScreen.tsx` uses the system share sheet, Storage Access Framework and document picker only                                                                                                      |
| Deletion available to users                         | `src/store/dhikrStore.ts` — `resetAll` clears all local tables; deletion requests by email                                                                                                                                                                                            |
