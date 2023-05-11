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

#### Data file

```
/data/data/[app_identifier]
```

#### Bypass SSL Pinning&#x20;

```bash
on android:
/data/local/tmp/frida-server &

on kali:
frida -v
objection --gadget [app_identifier] explore
android sslpinning disable
```
