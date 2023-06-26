# Android

#### APKleaks

```
apkleaks -f [path_to_apk].apk
```

#### AVD Emulator

```
sdkmanager --list
sdkmanager [package_name]
sdkmanager "platforms;android-28"
sdkmanager "system-images;android-28;default;x86"
sdkmanager "build-tools;28.0.3"
avdmanager create avd --name [avd_name] --package "system-images;android-28;default;x86"
emulator -avd [avd_name]    or    emulator -avd [avd_name] -writable-system -no-snapshot
emulator -list-avds

adb devices
adb -e push "KAD KAHWIN DIGITAL.apk" /storage/emulated/0/Download/KAD.apk
adb shell cmd package list packages -3        # list all third party app packages
```

#### Data file

```
/data/data/[app_identifier]
```

#### Bypass SSL Pinning

```bash
on android:
/data/local/tmp/frida-server &

on kali:
frida -v
objection --gadget [app_identifier] explore
android sslpinning disable
```

#### Reverse Engineering

```
1. Unzip jadx [https://github.com/skylot/jadx]
2. Run jadx-gui
3. Select apk file
4. Source code -> com -> [app_identifier] -> MainActivity
```
