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

## AVD Emulator

### SELinux

When an AVD is invoked, without the undermentioned applied, `gnome-abrt` shall inform the user that `qemu-system-x86` has `SIGABRT`'d. To prevent this, apply it:

~~~sh
#!/usr/bin/env sh
sudo ausearch -c 'RenderThread' --raw | audit2allow -M my-RenderThread && \
sudo semodule -X 300 -i my-RenderThread.pp
~~~

For a security-sensitive topic like this, you can install `sealert`, which shall appear when `gnome-abrt` does, and inform the user that this approach is correct.

### On copy-on-write filesystems

It's recommended to disable the copy-on-write behavior of the AVD Manager storage folder in order to run Android Virtual Devices without performance penalties. Most filesystems with this behavior (eg. BTRFS, ZFS, Bcachefs, etc.) support changing this configuration on a per-folder basis:

```sh
#!/usr/bin/env sh
chattr +C ~/.var/app/com.google.AndroidStudio/config/.android/avd
```

> [!IMPORTANT]
> This should only be done once, before creating any virtual devices. AVDs created prior won't be affected.

## LLVM

If you need LLVM for flutter app or something else you can install [`SDK Extension for LLVM Project 20`](https://github.com/flathub/org.freedesktop.Sdk.Extension.llvm20) and link to Android Studio,
required `build-options` are already present. You can use this commands:

```sh
#!/usr/bin/env sh
flatpak install flathub org.freedesktop.Sdk.Extension.llvm20 && \
flatpak override --user com.google.AndroidStudio --env=FLATPAK_ENABLE_SDK_EXT="llvm20"
```
