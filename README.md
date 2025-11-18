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

Some OSes (at least, Fedora and RHEL) enable SELinux, which restricts what unusual actions applications can perform without previous authorisation, in order to improve security. However, complex applications, like Android Studio's AVD Manager, need to be able to bypass this, in order to perform such low-level actions. Unfortunately, Flatpak does not yet support configuring this automatically, so the user must.

On these OSes, when an AVD is invoked without the override active, `gnome-abrt` shall inform the user that `qemu-system-x86` has `SIGABRT`'d. To prevent this, apply the undermentioned override:

```sh
#!/usr/bin/env sh
sudo ausearch -c 'RenderThread' --raw | audit2allow -M my-RenderThread && \
sudo semodule -X 300 -i my-RenderThread.pp
```

Because this is a matter of security, on Fedora, the user can install `sealert`, which shall appear when `gnome-abrt` does, and inform the user that this approach is correct. It is an official Red Hat tool.

Alternatively, use `setsebool` to set the SELinux boolean for only the current boot session (this might be advantageous security-wise if you don't need the AVD Manager often):

```sh
sudo setsebool selinuxuser_execheap true
```
Or if it is preffered that the boolean persist across reboots, use the `-P` option:

```sh
sudo setsebool -P selinuxuser_execheap true
```

### On copy-on-write filesystems

It's recommended to disable the copy-on-write behavior of the AVD Manager storage folder in order to run Android Virtual Devices without performance penalties. Most filesystems with this behavior (eg. BTRFS, ZFS, Bcachefs, etc.) support changing this configuration on a per-folder basis:

```sh
#!/usr/bin/env sh
chattr +C $HOME/.var/app/com.google.AndroidStudio/config/.android/avd
```

> [!IMPORTANT]
> This should only be done once, before creating any virtual devices. AVDs created prior won't be affected.
