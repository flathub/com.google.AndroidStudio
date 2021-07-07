# Android Studio

## Installing Android Studio Stable
```
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak install flathub com.google.AndroidStudio
```

## Installing Android Studio Canary
```sh
flatpak remote-add flathub-beta https://flathub.org/beta-repo/flathub-beta.flatpakrepo
flatpak install flathub-beta com.google.AndroidStudio
```

## Open Android Stable
```sh
flatpak run --branch=stable com.google.AndroidStudio
```

## Open Android Canary
```sh
 flatpak run --branch=beta com.google.AndroidStudio
```

## Bundle Java 9+
There are some approach :
1. Settings > Build, Execution, Deployment > Build Tools > Gradle > Select Gradle JDK to download (For Canary Only)
2. Install ``org.freedesktop.Sdk.Extension.OpenJdk9`` Flatpak Extension, and then set ``JAVA_HOME`` to ``/lib/sdk/openJdk9``
