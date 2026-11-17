# ios-xcrun-shim

A lightweight `xcrun` compatibility shim designed for on-device compilation on iOS (rootless Procursus/Dopamine environments).

## Why This Exists
When building software packages originally written for macOS or Darwin directly on iOS, build systems (`configure`, `CMake`, `setuptools`, `meson`, etc.) invariably call:
- `xcrun --show-sdk-path`
- `xcrun --show-sdk-version`
- `xcrun -f codesign_allocate`
- `xcrun codesign -s - --entitlements ...`

Because genuine Xcode CLI tools are unavailable on an iOS device, these calls fail immediately or cause build scripts to abort. 

`ios-xcrun-shim` intercepts these invocations:
- Returns appropriate local rootless SDK paths (`/var/jb/usr/share/SDKs/...`).
- Transparently converts `codesign` calls with entitlements into `ldid -S<entitlements>` pseudo-signatures required for iOS execution.
- Passes through commands smoothly so Darwin builds succeed natively on-device.

## Installation
```bash
cp xcrun /var/jb/usr/bin/xcrun
chmod +x /var/jb/usr/bin/xcrun
ln -sf ../../bin/xcrun /var/jb/usr/local/bin/xcrun
```
Or install the prebuilt `.deb` package:
```bash
dpkg -i xcrun_1.0.0_iphoneos-arm64.deb
```

## Author
Developed by [badger200](https://github.com/badger200).
