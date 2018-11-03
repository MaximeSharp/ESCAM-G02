# ESCAM-G02
DEBUG_FIRMWARE_ESCAM_G02 for failed updates (losing features, like ONVIF or web interface)  
`#Created | 2017/11/23`  
`#Updated | 2018/11/01 | Updated for last version of CamHi Application/Firmware`

#### Prerequisite
- **The softbrick G02 must have the P2P option activated, otherwise, you can't access it with the CamHi app.**  
- **The softbrick G02 must be accessible with CamHi app (or HiP2P Client on PC).**

### STEP 1 - Download the working firmware
- ESCAM Firmware repo : `http://47.91.141.20/`  
- When pressing the update button (on CAMHI app), this url will be requested : `http://47.91.141.20/goke_update.html`  
- And return the following string :  

```
{"list": [
{"url":"http://47.91.141.20/","ver":"V9.1.4.1.25"},
{"url":"http://47.91.141.20/","ver":"V9.1.5.1.11"},
{"url":"http://47.91.141.20/","ver":"V9.1.6.1.24"},
{"url":"http://47.91.141.20/","ver":"V9.1.11.1.7"},
{"url":"http://47.91.141.20/","ver":"V9.1.12.1.17"},
{"url":"http://47.91.141.20/","ver":"V9.1.14.1.7"},
{"url":"http://47.91.141.20/","ver":"V9.1.17.1.4"},
{"url":"http://47.91.141.20/","ver":"V11.1.8.1.4"},
{"url":"http://47.91.141.20/","ver":"V13.1.3.7.10"},
{"url":"http://47.91.141.20/","ver":"V13.1.6.1.26"},
{"url":"http://47.91.141.20/","ver":"V13.1.8.1.20"},
{"url":"http://47.91.141.20/","ver":"V13.1.9.7.12"},
{"url":"http://47.91.141.20/","ver":"V13.1.10.7.12"}]}
```

- Copy the content of this file to a notepad, we will need it later  
- For our ESCAM G02, we need this firmware version : `V13.1.6.1.26`  
- To download a firmware from their server, we need to use the following request : `http://47.91.141.20/{firmware_version}.exe`  
- So, in our case, we need to use that request : `http://47.91.141.20/V13.1.6.1.26.exe`  

#### STEP 2 - SETTING UP AN APACHE SERVER & CONFIGURATION 
- Download and install any Apache server (uwamp, xampp, mamp, etc..)  
- Create a file named `goke_update.html` on the server root folder, and paste the content you just copy from `STEP 1`  
- First, you need to replace the URL tag with **your** IP address. It can be easely done with the function Search and Replace in  [Notepad++](https://notepad-plus-plus.org/) (In my case, from `http://47.91.141.20/` to `http://192.168.1.15/G02/`)
- To force the IPCAM update, we need to modify the version number from `V13.1.6.1.26` to `V13.1.6.1.28` (or any higher value)    

Before | After
--|--
{"list": [{"url":"http://47.91.141.20/","ver":"V9.1.4.1.25"}, {"url":"http://47.91.141.20/","ver":"V9.1.5.1.11"}, {"url":"http://47.91.141.20/","ver":"V9.1.6.1.24"}, {"url":"http://47.91.141.20/","ver":"V9.1.11.1.7"}, {"url":"http://47.91.141.20/","ver":"V9.1.12.1.17"}, {"url":"http://47.91.141.20/","ver":"V9.1.14.1.7"}, {"url":"http://47.91.141.20/","ver":"V9.1.17.1.4"}, {"url":"http://47.91.141.20/","ver":"V11.1.8.1.4"}, {"url":"http://47.91.141.20/","ver":"V13.1.3.7.10"}, **{"url":"http://47.91.141.20/","ver":"V13.1.6.1.26"},** {"url":"http://47.91.141.20/","ver":"V13.1.8.1.20"}, {"url":"http://47.91.141.20/","ver":"V13.1.9.7.12"}, {"url":"http://47.91.141.20/","ver":"V13.1.10.7.12"}]} | {"list": [{"url":"http://192.168.1.15/G02/","ver":"V9.1.4.1.25"}, {"url":"http://192.168.1.15/G02/","ver":"V9.1.5.1.11"}, {"url":"http://192.168.1.15/G02/","ver":"V9.1.6.1.24"}, {"url":"http://192.168.1.15/G02/","ver":"V9.1.11.1.7"}, {"url":"http://192.168.1.15/G02/","ver":"V9.1.12.1.17"}, {"url":"http://192.168.1.15/G02/","ver":"V9.1.14.1.7"}, {"url":"http://192.168.1.15/G02/","ver":"V9.1.17.1.4"}, {"url":"http://192.168.1.15/G02/","ver":"V11.1.8.1.4"}, {"url":"http://192.168.1.15/G02/","ver":"V13.1.3.7.10"}, **{"url":"http://192.168.1.15/G02/","ver":"V13.1.6.1.28"}**, {"url":"http://192.168.1.15/G02/","ver":"V13.1.8.1.20"}, {"url":"http://192.168.1.15/G02/","ver":"V13.1.9.7.12"}, {"url":"http://192.168.1.15/G02/","ver":"V13.1.10.7.12"}]}

- Save and exit, and then, copy the firmware file `V13.1.6.1.26.exe` and rename it with the same version number that you define in the  file `goke_update.html` (In my case, `V13.1.6.1.28.exe`)  

### STEP 3 - CAMHI APP -> EXTRACT, DECOMPILE, EDIT, RECOMPILE 
- Download CamHi on the Playstore, and extract the APK with an app (like APK Extractor)
- Copy the APK on your computer
- Download and extract [Advanced ApkTool (v4.2.0)](https://forum.xda-developers.com/showpost.php?p=77638764&postcount=835)  
- Decompile the APK and navigate to the following folder :  
>/smali/com/hichip/thecamhi/activity/setting/

- Open `SystemSettingActivity.smali` with an editor (like [Notepad++](https://notepad-plus-plus.org/))
- Go to the line 28    
- Edit  `http://47.91.141.20/goke_update.html` to `http://{your_apache_address}/goke_update.html`  
>ex: http://192.168.1.15/G02/goke_update.html

- Save your edit, recompile the APK, and install it on your smartphone
- Open the app, add the softbrick G02, navigate to `System Settings` and `CHECK FOR UPDATES`  
- If you do everything below this message, the app should popup a message saying that a new firmware is available, and asking you if you want to update. Click on `OK`, and let the G02 proceed to the update (When finished, the G02 should reboot automatically)

##### AND HERE WE GO, YOUR ESCAM G02 WORK GREAT AGAIN !
###### If you encountered some issues :  
- ###### If the app tell you that the G02 is up to date, you've done something wrong. Here some reasons :  
 	- In **`STEP 2`**, you've probably edit the wrong version number in **`goke-update.html`** file : Edit it and test again.  
- ###### If the app tell 'Connection Server Failed :  
	- In **`STEP 2`**, you've probably put the files in the wrong folder : Follow **`STEP 2`** again.
 	- In **`STEP 3`**, you've probably put the wrong IP address in **`SystemSettingActivity.smali`** : Follow **`STEP 3`** again.
- ###### If the update failed :  
 	- In **`STEP 2`**, you've probably edit the wrong version number in the **`exe`**  file : Edit it and test again.
