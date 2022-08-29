# Android Pentest
## Extract APK
```
1. Backup APK.
2. SCP/SSH to copy out the APK file.
```

### Static Analysis
```
python cli.py axml android.apk    # decrypt manifest.xml
python cli.py arsc android.apk    # decrypt resources.arsc
REF: https://github.com/androguard/androguard

apkurlgrep -a android.apk   # grep all urls in the apk
REF: https://github.com/ndelphit/apkurlgrep
```
