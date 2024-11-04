# Android Studio Flatpak wrapper
This is a community made Flatpak wrapper of Android Studio, it's not verified by, affiliated with, or supported by Google.
This wrapper uses x11 or xwayland due to openJDK missing support. Filesystem visibility is voluntarily limited to home and flatpak-spawn not permitted due to security reason.

Any suggestions, problem reports or improvement proposals are welcome.

## Android Udev rules
Many systems need UDEV rules to handle USB devices, we suggest these [android-udev-rules](https://github.com/M0Rf30/android-udev-rules) 

## ADB systemd service
If you want to start ADB Server Daemon on user login (and not on Android Studio boot) this is a systemd service example.
To disable management by Android studio go to `Settings`, `Build, Execution, Deployment`, `Debugger`, `ADB server lifecycle management` and select `Use existing manually managed server`

```
[Unit]
Description=Android Debug Server Daemon

[Service]
Type=forking
ExecStart=%h/android/sdk/platform-tools/adb start-server
ExecStop=%h/android/sdk/platform-tools/adb kill-server

[Install]
WantedBy=default.target
```
