# RaiApp — SCAFFOLD-ENG

Flutter English learning app with Android, iPhone, macOS, and web project files.
Use Flutter 3.41.5 (Dart 3.11.3), matching the committed dependency lockfile.
The Laravel API is a separate project and is not included here.

## Build an iPhone IPA from Windows

1. Open [Build iPhone IPA](https://github.com/philippradodjx01/IPA_Testing/actions/workflows/dart.yml).
2. Select **Run workflow**, enter the Laravel API URL reachable from your phone (including `/api/v1`), and start the run.
3. Open the completed run and download **RaiApp-unsigned-iphone** from **Artifacts**.
4. Extract the downloaded ZIP to get `RaiApp-unsigned.ipa`.

GitHub builds on a macOS runner; Windows does not need Xcode. Pushes to the app or iOS workflow also build automatically. Automatic builds use the repository variable `API_BASE_URL`, falling back to `http://192.168.0.61:8000/api/v1`.

### Install on your own iPhone

The IPA is **unsigned**. It cannot be installed by simply opening it on your iPhone, and it is not ready for TestFlight or the App Store.

Use [AltStore Classic's official Windows setup](https://faq.altstore.io/altstore-classic/how-to-install-altstore-windows) to install AltServer and AltStore on your phone. Follow its Apple software, device trust, and Developer Mode instructions. Transfer the extracted IPA to the iPhone, then use **My Apps → +** in AltStore to select it and sign/install it with your Apple account. Keep Apple credentials in the signing tool; do not put them in this repository or GitHub Actions. Free-account installs require periodic refresh; follow AltStore's current account limits and refresh instructions.

The Windows computer running Laravel and the iPhone must be reachable over the same network for a LAN API URL. Start the backend in its own project with:

```sh
php artisan serve --host=0.0.0.0 --port=8000
```

Allow the backend through your Windows firewall on your private network. Accept the app's local-network permission on iPhone. Replace the example IP with your computer's current LAN address. For access away from home, use a deployed HTTPS API and rebuild with that URL. The API URL is included in the app and must not contain secrets.

## Run on a Mac later

Install Flutter 3.41.5 and Xcode using [Flutter's macOS setup](https://docs.flutter.dev/platform-integration/macos/setup), then:

```sh
flutter pub get
flutter run -d macos --dart-define=API_BASE_URL=https://your-server.example/api/v1
```

The macOS app includes the network-client sandbox entitlement needed to reach the API. Apple builds must be compiled on macOS.

For a signed iPhone archive on a Mac, configure a unique bundle ID and your Apple signing team in Xcode, then follow [Flutter's iOS deployment guide](https://docs.flutter.dev/deployment/ios) and run `flutter build ipa` with your API URL. Native pronunciation is currently unavailable; the existing pronunciation implementation supports web only.
