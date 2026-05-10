# Installing Dusk on iOS via SideStore

## Prerequisites
- Computer
- iPhone connected via USB
- Dusk IPA file (download the latest `Dusk-vX.X.X-ios-arm64.ipa` from the [releases page](https://github.com/TwilitRealm/dusk/releases))
- Game disc ROM file - `GZ2E01` (Gamecube USA) or `GZ2PE01` (Gamecube PAL)

## 1. Install SideStore
Follow the [Official SideStore Installation Guide](https://docs.sidestore.io/docs/installation/prerequisites) to get the SideStore app running on your device.

## 2. Enable Developer Mode (iOS 16+)
- On your iPhone, go to **Settings > Privacy & Security > Developer Mode**
- Toggle it on and restart when prompted.

## 3. Connect to SideStore VPN
- Install and open the **WireGuard** app on your iPhone.
- Turn on the **SideStore** VPN connection.
*(Note: SideStore requires this VPN to be active whenever you install or refresh apps)*

## 4. Copy Files to Your iPhone
Transfer the IPA and game disc to your iPhone so they're accessible in the Files app. A few ways to do this:
- **AirDrop** - Right-click the files on your Mac and choose Share > AirDrop.
- **iCloud Drive** - Place files in iCloud Drive on your Mac and they'll sync to Files on your iPhone.
- **USB transfer** - Connect your iPhone and drag files via Finder's sidebar.
- **Cloud storage** - Upload to Google Drive, Dropbox, etc., and download on your iPhone.

## 5. Install via SideStore
- Open **SideStore** on your iPhone.
- Go to the **My Apps** tab.
- Tap the **+** button (top left).
- Open the **Files** app and select the `.ipa` file.

## 6. Load the Game Disc
- Open the **Dusk** app once to allow it to create its local folders.
- Open the **Files** app and navigate to **On My iPhone**.
- Open the **Dusk** folder.
- Move your game disc ROM file into this folder.
- Relaunch **Dusk** to begin your journey!

---

> **Pro-Tip:** SideStore apps expire every **7 days**. To ensure you don't lose access, remember to connect to your WireGuard VPN and open SideStore to **Refresh** your apps once a week.
