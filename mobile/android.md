# Android

#### APKleaks

```
apkleaks -f [path_to_apk].apk
```

#### AVD Emulator

```
sdkmanager --list
sdkmanager [package_name]
avdmanager create avd --name [avd_name] --package "system-images;android-28;default;x86"
emulator -avd [avd_name]    or    emulator -avd [avd_name] -writable-system -no-snapshot
emulator -list-avds

```
