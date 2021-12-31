# iOS

## Rooting/Jailbreaking
#### Required tools
:wrench: [unc0ver 8.0.2](https://unc0ver.dev/) support iOS 11.0 - 14.8 (31/Dec/2021) <br />
:wrench: [Sideloadly](https://sideloadly.io/)<br />
```
Steps
1. Push the unc0ver app to phone using sideloadly.
2. Run the unc0ver app from the phone.
3. Wait for it to be done. 
4. Verify successfully rooted with any jailbreak apps.
```

## The basics
#### Finding the App's ID 
```
Installed apps data are stored in:
/var/mobile/Containers/data/Application

To find the app id, ssh into the phone and via terminal cd to the above mentioned directory.
My-iPhone:~ root# cd /private/var/mobile/Containers/Data/Application/
My-iPhone:/private/var/mobile/Containers/Data/Application root# find * | grep -i [application name]
```

#### Finding the sqlite file
```
cd into the application's data folder
My-iPhone:/private/var/mobile/Containers/Data/Application/F9D3E3F0-691F-4FAE-BCBC-81C9EFB10BA7 root# find * | grep -i sqlite
Library/Application Support/CoreData.sqlite-shm
Library/Application Support/CoreData.sqlite-wal
Library/Application Support/CoreData.sqlite
```
