# Running App on iPhone Device

## Option 1: Using TestFlight (Recommended for Testing)

This is the easiest way to test your app on your iPhone without needing to connect to your Mac.

### Prerequisites:
1. Fill in TestFlight beta information (if you haven't already):
   - Go to: https://appstoreconnect.apple.com/apps/6757089477/testflight/test-info
   - Fill in Feedback Email and Beta App Review Information

### Steps:

1. **Install TestFlight app** on your iPhone:
   - Open App Store on your iPhone
   - Search for "TestFlight"
   - Install the TestFlight app

2. **Accept TestFlight invitation**:
   - Add your Apple ID email as an internal tester in App Store Connect
   - Check your email for TestFlight invitation
   - Accept the invitation (or add yourself as tester in App Store Connect → TestFlight → Internal Testing)

3. **Install your app**:
   - Open TestFlight app on your iPhone
   - Find "Testing the ai" app
   - Tap "Install" to install the app
   - The app will appear on your home screen

4. **Test the app**:
   - Open the app from your home screen
   - TestFlight will show any available updates

**Note**: Each new build uploaded to TestFlight will automatically appear in the TestFlight app for update.

---

## Option 2: Direct Install via Xcode (For Development/Debugging)

Use this method if you want to build and install directly from your Mac with Xcode.

### Prerequisites:
1. Your iPhone must be connected to your Mac via USB (or connected to the same Wi-Fi network)
2. Xcode installed on your Mac
3. Your Apple ID signed in to Xcode
4. Trust your Mac on your iPhone (when prompted)

### Steps:

1. **Connect your iPhone**:
   - Connect iPhone to Mac via USB cable
   - Unlock your iPhone
   - If prompted, tap "Trust This Computer" on your iPhone

2. **Open the project in Xcode**:
   ```bash
   cd /Users/lebemac/Documents/Developers/flutter_app/test_ai
   open ios/Runner.xcworkspace
   ```
   Note: Open `.xcworkspace`, not `.xcodeproj`

3. **Select your iPhone as the destination**:
   - In Xcode, click on the device selector at the top (next to the Run button)
   - Select your iPhone from the list

4. **Configure code signing**:
   - Select the "Runner" target in the left sidebar
   - Go to "Signing & Capabilities" tab
   - Check "Automatically manage signing"
   - Select your Team (should show your Apple Developer account)
   - Bundle Identifier should be: `com.lebe24.testai`

5. **Build and run**:
   - Click the Play button (▶️) in Xcode, OR
   - From terminal, run:
     ```bash
     flutter run --release
     ```
   - Or for debug build:
     ```bash
     flutter run
     ```

6. **Trust the developer on iPhone** (first time only):
   - After installation, go to iPhone Settings → General → VPN & Device Management
   - Find your developer certificate
   - Tap "Trust [Your Name]"

---

## Option 3: Using Flutter Command Line

You can also build and install directly using Flutter commands:

### Steps:

1. **Connect your iPhone**:
   - Connect via USB or ensure both devices are on same Wi-Fi

2. **Check connected devices**:
   ```bash
   flutter devices
   ```
   You should see your iPhone listed

3. **Build and install**:
   ```bash
   flutter run --release
   ```
   Or for debug:
   ```bash
   flutter run
   ```

4. **Trust the developer** (if prompted):
   - On iPhone: Settings → General → VPN & Device Management
   - Trust your developer certificate

---

## Troubleshooting

### "No devices found"
- Make sure iPhone is unlocked
- Check USB cable connection
- Trust the computer on your iPhone
- Run `flutter devices` to verify

### "Code signing error"
- Make sure you're signed in to Xcode with your Apple ID
- Check that Bundle ID matches: `com.lebe24.testai`
- Verify your Developer account has proper permissions

### "Untrusted Developer"
- Go to iPhone Settings → General → VPN & Device Management
- Find your developer certificate
- Tap "Trust [Your Name]"

### Build fails with provisioning profile error
- In Xcode, go to Signing & Capabilities
- Make sure "Automatically manage signing" is checked
- Select your Team
- Xcode will automatically create/use the correct provisioning profile

---

## Current Configuration

- **Bundle ID**: `com.lebe24.testai`
- **Minimum iOS Version**: 13.0 (supports all modern iPhones)
- **Development Team**: 7TH5U8BA6U
- **App Name**: Test Ai
- **Xcode Version**: 14.2 (iOS 16.2 SDK)

## Xcode 14 Compatibility with Your iPhone

✅ **Xcode 14.2 will work** to deploy to your iPhone!

**Compatibility Notes:**
- **Xcode 14.2** supports iOS 16.2 SDK
- Your app's **deployment target is iOS 13.0**, which is compatible
- Xcode 14 can deploy apps to iPhones running:
  - ✅ iOS 16.x (fully supported)
  - ✅ iOS 17.x (should work, but some newer features unavailable)
  - ⚠️ iOS 18.x (may have limitations, but basic deployment should work)

**If you encounter issues:**
- Make sure your iPhone is unlocked and trusted
- Check that your Apple ID is signed in to Xcode
- Try using TestFlight instead (works regardless of Xcode version)

**Note**: If your iPhone is running iOS 18 or newer, you might want to update to Xcode 15 or later for better compatibility, but Xcode 14 should still work for basic deployment.

