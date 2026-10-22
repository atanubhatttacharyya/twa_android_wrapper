# Elevate TWA Android project

This is the Android Trusted Web Activity (TWA) project for the existing Elevate website. It does not contain or replace the Flask website source code.

- Website: https://notrade.myelevateapps.com
- Application ID: `com.elevate.optiontrading`
- App name: `Elevate Option Trading`
- Build: Codemagic cloud build; Android Studio is not required on your computer.

## Build a debug APK with Codemagic

1. Extract this ZIP.
2. Commit the contents of this folder (not an extra enclosing folder) to a new branch in `atanubhatttacharyya/notrade_android_wrapper`.
3. In Codemagic, connect the repository and select `codemagic.yaml`.
4. Run workflow `android-debug`.
5. Download `app-debug.apk` from build artifacts and install it on an Android phone.

## Important: domain association is still required

The website must serve `https://notrade.myelevateapps.com/.well-known/assetlinks.json` with this exact package ID and the SHA-256 fingerprint of the certificate that signed the installed APK:

- Package ID: `com.elevate.optiontrading`
- Certificate fingerprint: obtain from the actual Codemagic debug/release signing certificate. Do not copy a sample fingerprint.

For Play releases, use the Play App Signing certificate fingerprint. If the debug build uses a different signing certificate, add that fingerprint for testing only as appropriate.

## Production environment

The website's existing TWA endpoint must be configured with `ANDROID_TWA_PACKAGE_ID` and `ANDROID_TWA_SHA256_CERT_FINGERPRINTS`. Configure HTTPS and secure-cookie settings separately in the server environment. This project does not change AWS.

## Authentication and limitations

TWA uses the supported browser context for the existing website authentication. The APK build itself cannot guarantee Gmail login: test Google sign-in, the authenticated landing page, logout, session expiry and the eight-hour session policy on a physical Android device. Do not add a WebView-specific callback/session bridge to this TWA project unless testing establishes a genuine requirement.

## Signing

The debug workflow creates a test APK. The release workflow is intentionally unsigned; configure a secure signing key in Codemagic before a production release. Updating an already-installed app requires the same application ID and compatible signing key.

## What has not been done

- No GitHub push was performed.
- No Codemagic build was run from this environment.
- No live AWS configuration was changed.
- No Android device test was performed.
