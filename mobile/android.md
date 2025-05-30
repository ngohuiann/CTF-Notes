# Android

#### APKleaks

```
apkleaks -f [path_to_apk].apk
```

#### AVD Emulator

[https://medium.com/michael-wallace/how-to-install-android-sdk-and-setup-avd-emulator-without-android-studio-aeb55c014264](https://medium.com/michael-wallace/how-to-install-android-sdk-and-setup-avd-emulator-without-android-studio-aeb55c014264)

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
adb root
adb -e push "KAD KAHWIN DIGITAL.apk" /storage/emulated/0/Download/KAD.apk
adb pull /sdcard/Download/files.zip .
adb shell cmd package list packages -3        # list all third party app packages
adb kill-server
```

#### Data file

```
/data/data/[app_identifier]
```

#### Bypass SSL Pinning

```
1. Magisk
2. Deny List
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

#### R.java

The `R.java` file in an Android project is generated by the Android build system and contains resource identifiers for the various resources used in your application, such as layouts, strings, drawables, and more.

The mapping value can be found under resources.

resources.arsc -> res -> values -> strings.xml

#### Reflutter & Sign APK

```
reflutter [apk].apk
java -jar uber-apk-signer.jar --apks [refluttered].apk
```

### Android Studio

```kotlin
package com.example.myapplication

import android.os.Build.MODEL
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import com.example.myapplication.ui.theme.MyApplicationTheme


class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            MyApplicationTheme {
                Scaffold(modifier = Modifier.fillMaxSize()) { innerPadding ->
                    Greeting(

                        name = MODEL,
                        modifier = Modifier.padding(innerPadding)
                    )
                }
            }
        }
    }
}

@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Text(
        text = "Hello $name!",
        modifier = modifier
    )
}

@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    MyApplicationTheme {
        Greeting("Android1")
    }
}
```
