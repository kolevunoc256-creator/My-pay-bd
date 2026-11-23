# MyPay BD — Flutter starter

This ZIP contains the Flutter source code for a MyPay BD starter app:
- Purple/pink dashboard UI and sample transaction list
- Firebase phone-number login and SMS OTP verification flow
- Auth state gate and logout
- Firestore rules that deny all client access by default

## Important limitations

This is a development starter, not a licensed mobile financial service. The dashboard still displays demo balance/transactions and Send Money/Cash Out are demo interactions only. No real money is transferred. No payment provider is connected, no backend ledger is deployed, and no production security/compliance review has been completed.

Real-money functionality requires an approved payment provider, a server-side transaction ledger, server-side validation and idempotency, monitoring/audit logs, KYC/AML processes where applicable, and security review. Never store secret API keys in the Flutter app. Do not loosen Firestore rules to make client-side balance updates work.

## 1. Install tools

Install Flutter SDK and Android Studio. Open a terminal in this project folder and run:

```bash
flutter doctor
flutter pub get
```

## 2. Create/configure Firebase

1. Open https://console.firebase.google.com/ and create a Firebase project.
2. Add an Android app using your actual Android application ID. If you have not created Android platform files yet, run `flutter create .` from this project folder first.
3. Download `google-services.json` and put it in `android/app/google-services.json`.
4. In Firebase Console, open Authentication -> Sign-in method -> enable Phone.
5. Add your app's SHA-1 and SHA-256 fingerprints in Firebase project settings. For local testing, use the debug signing certificate fingerprints.
6. Configure the Google Services Gradle plugin by following the current Firebase Android setup guide: https://firebase.google.com/docs/flutter/setup
7. For iOS, add `GoogleService-Info.plist` to the Runner target and follow Firebase's current Flutter setup guide.
8. In Firebase Authentication settings, configure allowed SMS regions and app verification settings. Use Firebase test phone numbers during development to avoid sending real SMS repeatedly.

Firebase phone auth may require billing, quota/region configuration, app verification, and valid platform setup. Availability and SMS delivery can vary by country/carrier.

## 3. Run the app

```bash
flutter run
```

If Firebase is not configured correctly, the app will show a Firebase setup message rather than opening the login screen.

## 4. Build an Android APK

After Firebase is configured and the app runs successfully:

```bash
flutter build apk --release
```

APK output:
`build/app/outputs/flutter-apk/app-release.apk`

To install on a connected Android phone:
```bash
flutter install
```

## 5. Firestore rules

The included `firestore.rules` denies all direct client reads and writes. This is intentional. Deploy it with Firebase CLI only after installing/configuring Firebase CLI:

```bash
firebase login
firebase use --add
firebase deploy --only firestore:rules
```

Do not store or update a wallet balance from the Flutter client. A production backend must own the ledger and validate every transaction.

## What you must provide/configure yourself

- Firebase project configuration (`google-services.json`; iOS plist if needed)
- Android package/application ID and signing fingerprints
- Approved payment provider credentials and merchant approval, if real payments are planned
- Backend hosting and production security/compliance review

The source ZIP does not contain credentials or a built APK.


## Build an APK using only an Android phone (GitHub Actions)

You can trigger a cloud build from Chrome on Android; a computer is not required.

1. Create/sign in to a GitHub account in Chrome.
2. Create a new **private** repository named `mypay-bd`.
3. Upload the contents of this ZIP into the repository (extract the ZIP first; include the `.github` folder).
4. Open the repository -> **Actions** -> **Build MyPay BD APK**.
5. Tap **Run workflow** and wait for the run to finish successfully.
6. Open the completed run, scroll to **Artifacts**, and download `mypay-bd-apk`.
7. Extract the downloaded artifact ZIP; install `app-release.apk`. Android may ask you to allow installation from that browser/file manager.

This cloud workflow builds a demo-preview APK. Without your Firebase Android configuration, the app opens a Firebase setup page with an option to preview the demo dashboard; live SMS OTP is not active. Configure Firebase before expecting OTP to work.
