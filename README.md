# Android Studio Flatpak Wrapper

This is a community-made Flatpak wrapper of Android Studio. It's not verified by, affiliated with, or supported by Google.

This wrapper uses X11 via XWayland, due to missing support in OpenJDK. Filesystem visibility is voluntarily limited to home, and `flatpak-spawn` is not permitted because it would reduce security.

Any suggestions, problem reports, or improvement proposals are welcome.

## Android `udev` Rules

Many systems need `udev` rules to handle USB devices, we suggest these: [`android-udev-rules`](https://github.com/M0Rf30/android-udev-rules).

## ADB SystemD Service

If you want to start ADB Server Daemon on user login (and not on Android Studio boot), this is a SystemD service example.

To disable management by Android Studio go, to:

1. "Settings",
2. "Build, Execution, Deployment",
3. "Debugger", and
4. "ADB server lifecycle management".

Then, select "Use existing manually managed server".

```desktop
[Unit]
Description=Android Debug Server Daemon

[Service]
Type=forking
ExecStart=%h/android/sdk/platform-tools/adb start-server
ExecStop=%h/android/sdk/platform-tools/adb kill-server

[Install]
WantedBy=default.target
```
