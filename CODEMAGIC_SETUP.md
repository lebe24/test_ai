# Codemagic TestFlight Setup Guide

## Goal
Use TestFlight for easy device testing. TestFlight requires **App Store Distribution** provisioning profiles (even though it's for testing).

## Current Issue
The build is using a **Development provisioning profile** when it should use an **App Store Distribution provisioning profile** for TestFlight.

## Solution: Configure Codemagic for TestFlight

### 1. App Store Connect Setup

First, ensure your app is set up in App Store Connect:

1. Go to [App Store Connect](https://appstoreconnect.apple.com)
2. Create a new app (if not already created):
   - **Bundle ID**: `com.lebe24.testai`
   - **App Name**: Test Ai
   - **Primary Language**: Your preferred language

### 2. Codemagic Code Signing Configuration

In Codemagic, configure code signing for App Store Distribution:

1. Go to your Codemagic app → **Code signing**
2. **iOS code signing identity**: Set to **Distribution** (not Development)
3. **Provisioning profile type**: Set to **App Store** (not Development or Ad Hoc)
4. If using **automatic signing**, ensure:
   - App Store Connect API key has **App Manager** or **Admin** role
   - Key ID: `6G9FR8RFNU` (your current key)
   - The key can create/use Distribution certificates and profiles

### 3. Verify App Store Connect API Key

Your API key: `6G9FR8RFNU`

Check permissions:
1. Go to [App Store Connect → Users and Access → Keys](https://appstoreconnect.apple.com/access/api)
2. Find key ID `6G9FR8RFNU`
3. Ensure it has:
   - **App Manager** or **Admin** role
   - Permissions to create Distribution certificates
   - Permissions to create App Store Distribution provisioning profiles

### 4. Build Configuration

Your Codemagic build script should use:
```bash
flutter build ipa --release
```

**Keep the `app-store-connect publish` step** - this uploads to TestFlight:
```bash
app-store-connect publish \
  --path build/ios/ipa/test_ai.ipa \
  --key-id 6G9FR8RFNU \
  --issuer-id e5e3f2cd-e027-49b9-9a90-d5494f1d7699 \
  --private-key @env:APP_STORE_CONNECT_PUBLISHER_PRIVATE_KEY
```

### 5. Manual Provisioning Profile Setup (If Automatic Fails)

If automatic code signing doesn't work:

1. **Create App Store Distribution Certificate**:
   - Go to [Apple Developer → Certificates](https://developer.apple.com/account/resources/certificates/list)
   - Create new **Apple Distribution** certificate
   - Download the certificate (.cer file)
   - Convert to .p12 and upload to Codemagic

2. **Create App Store Distribution Provisioning Profile**:
   - Go to [Apple Developer → Profiles](https://developer.apple.com/account/resources/profiles/list)
   - Create new profile for **App Store** distribution
   - Select App ID: `com.lebe24.testai`
   - Select the Distribution certificate
   - Download the provisioning profile (.mobileprovision file)
   - Upload to Codemagic

3. **Configure in Codemagic**:
   - Upload Distribution certificate (.p12 file)
   - Upload App Store Distribution provisioning profile (.mobileprovision file)
   - Set code signing to use these files manually

## Verification

After configuring correctly, the build log should show:
- ✅ **Distribution type**: `App Store` (not Development)
- ✅ **Provisioned devices**: Empty (App Store profiles don't have device restrictions)
- ✅ **Certificate type**: `Apple Distribution`
- ✅ **Upload succeeds** to App Store Connect

## TestFlight Distribution

Once uploaded successfully:

1. **Wait for processing**: App Store Connect will process the build (usually 5-15 minutes)
2. **Add to TestFlight**: Go to App Store Connect → TestFlight
3. **Add internal testers**: Add your Apple ID email as an internal tester
4. **Install TestFlight app**: Install TestFlight from App Store on your device
5. **Accept invitation**: Accept the TestFlight invitation email
6. **Install your app**: Open TestFlight app and install your app

## Important Notes

- **TestFlight uses App Store Distribution profiles** - this is correct even for testing
- **No device UDIDs needed** - App Store profiles work on any device (via TestFlight)
- **Build will be processed** - First upload takes longer as App Store Connect processes it
- **Version number must increment** - Each TestFlight upload needs a new version number

## Current Project Configuration

✅ Xcode project is correctly configured:
- Release configuration uses `Apple Distribution` code signing
- Profile configuration uses `Apple Distribution` code signing
- Archive action uses Release configuration
- Bundle ID: `com.lebe24.testai`

The issue is in **Codemagic's code signing setup** - ensure it's using App Store Distribution provisioning profiles, not Development profiles.
