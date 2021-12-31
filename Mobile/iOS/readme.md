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

## [OWASP iGoat](https://github.com/OWASP/igoat)
### Data Protection (Rest)
#### Core Data Storage
```
Steps (basically what is mentioned above):
1. Locate application id and its data folder.
2. Copy the folder from iPhone to PC through winscp.
3. Open the CoreData.sqlite file.
4. Find the email and password.
```
![image](https://user-images.githubusercontent.com/31241187/147802735-064afb58-b738-47ef-af07-4b398f81d654.png)
![image](https://user-images.githubusercontent.com/31241187/147802711-bea6b86b-6147-491c-8cdf-6fedab983b69.png)
![image](https://user-images.githubusercontent.com/31241187/147802693-d30b628b-2d40-4653-9782-3fb2ce005875.png) <br />
<img src="https://user-images.githubusercontent.com/31241187/147802913-0b32eee0-52f1-4efc-94d7-149cfa02981a.png" width="200" height="350" />
<img src="https://user-images.githubusercontent.com/31241187/147802942-b0186512-2a95-4a75-a04f-f203734d1c4b.png" width="200" height="350" />
