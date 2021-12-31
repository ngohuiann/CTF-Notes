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

## [OWASP iGoat](https://github.com/OWASP/igoat) Writeup
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

#### Plist Storage
```
Steps
1. Locate Credentials.plist in the data folder.
2. The plist file can be open with basic notepad file 
OR
2. plistutil -i [plistfile] -o [outfile].xml    # in linux
```
![image](https://user-images.githubusercontent.com/31241187/147804826-43cc3040-2142-44df-86de-8d5a4f732c65.png)
<img src="https://user-images.githubusercontent.com/31241187/147804913-55a589c4-6ec7-470a-8fc5-0b9980196e17.png" width="200" height="350" />
<img src="https://user-images.githubusercontent.com/31241187/147804950-d34b683d-b2d9-48fd-a97c-924d2f06609f.png" width="200" height="350" />

#### NSUserDefaults Storage
```
Steps
1. Locate the plist file.
2. Convert the file to xml with plistutil.
```
![image](https://user-images.githubusercontent.com/31241187/147812068-a9bedd28-167f-46dd-8bab-a78b62222e92.png)
<img src="https://user-images.githubusercontent.com/31241187/147812215-8747d451-4915-482f-b938-cf5040a05069.png" width="200" height="350" />

### Injection Flaws
#### Cross Site Scripting
```
Steps
1. Enter any data into the input field and load the data.
2. Observe the input data was being reflected on the UIWebView.
3. Insert xss code.
4. The injected script was triggered and reflected.
```
<img src="https://user-images.githubusercontent.com/31241187/147807087-fdc2eba4-1d81-455e-bf4a-ecdc7d2e2543.png" width="200" height="350" /> <img src="https://user-images.githubusercontent.com/31241187/147807064-067921b7-b2cf-4321-8989-03be100cd5de.png" width="200" height="350" />

