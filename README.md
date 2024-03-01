# Android Studio Flatpak

🚨 Warning: This is an unofficial Flatpak build of Android Studio, generated from the official Google-built zip file [here](https://github.com/flathub/com.google.AndroidStudio/blob/29cb2786d0178fddf2d4a6c2b58b036ca8825f2d/com.google.AndroidStudio.json#LL54C26-L54C26).

## Usage

Most functionality works out of the box, though please note that flatpak runs in an isolated environment and some work is necessary to enable those features.

### Execute commands in the host system.

To execute commands on the host system, run inside the sandbox:

`$ flatpak-spawn --host <COMMAND>`
