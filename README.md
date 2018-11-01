# ESCAM-G02
DEBUG_FIRMWARE_ESCAM_G02 for failed updates (losing features, like ONVIF or web interface) #edit 2017/11/23

## STEP 1 - Download the working firmware

ESCAM Firmware repo : http://47.91.141.20/
When pressing the update button (on CAMHI app), this url will be requested : http://47.91.141.20/goke_update.html
And return a JSON(?) string : 

>{"list":[
>{"url":"http://47.91.141.20/","ver":"V9.1.4.1.25"},
>{"url":"http://47.91.141.20/","ver":"V9.1.5.1.11"},
>{"url":"http://47.91.141.20/","ver":"V9.1.6.1.24"},
>{"url":"http://47.91.141.20/","ver":"V9.1.11.1.7"},
>{"url":"http://47.91.141.20/","ver":"V9.1.12.1.17"},
>{"url":"http://47.91.141.20/","ver":"V9.1.14.1.7"},
>{"url":"http://47.91.141.20/","ver":"V11.1.8.1.4"},
>{"url":"http://47.91.141.20/","ver":"V13.1.6.1.10"}
>]

Copy the content of this file to a notepad, we will need it later

For our ESCAM G02, we need this firmware version : V9.1.6.1.24
To download a firmware from their server, we need to use the following request: http://47.91.141.20/{firmware_version}.exe
So, in our case, we need to use that request : >http://47.91.141.20/V9.1.6.1.24.exe                                          

## STEP 2 - SETTING UP AN APACHE SERVER & CONFIGURATION
 
DOWNLOAD AND INSTALL APACHE SERVER (ex:WAMP,LAMP,MAMP,...)

PLACE AT THE WWW FOLDER THE FILES "goke_update.html" AND "V9.1.6.1.24.exe"

RENAME THE FILE "V9.1.6.1.24.exe" TO "V9.1.6.1.25.exe"

OPEN AND EDIT THE FILE "goke_update.html" :
 
>{"list": [
>{"url":"http://47.91.141.20/","ver":"V9.1.4.1.25"},
>{"url":"http://47.91.141.20/","ver":"V9.1.5.1.11"},
>{"url":"http://47.91.141.20/","ver":"V9.1.6.1.25"},
>{"url":"http://47.91.141.20/","ver":"V9.1.11.1.7"},
>{"url":"http://47.91.141.20/","ver":"V9.1.12.1.17"},
>{"url":"http://47.91.141.20/","ver":"V9.1.14.1.7"},
>{"url":"http://47.91.141.20/","ver":"V11.1.8.1.4"},
>{"url":"http://47.91.141.20/","ver":"V13.1.6.1.10"}
>]}

## STEP 3 - CAMHI APP -> EXTRACT, DECOMPILE, EDIT, RECOMPILE 

EXTRACT YOUR CAMHI APK AND SAVE IT ON YOUR COMPUTER

DOWNLOAD AND INSTALL APKTOOL (OR ADVANCED APKTOOL ON WINDOWS)

DECOMPILE YOUR APK AND NAVIGATE TO THIS FILE : 

>/smali/com/thecamhi/activity/setting/SystemSettingActivity.smali

OPEN IT AND GO TO THE LINE 73

EDIT

>"http://47.91.141.20/goke_update.html"

TO 

>"http://{server_ip_address}/goke_update.html"

>ex: http://192.168.1.19/goke_update.html"
                                                
SAVE AND RECOMPILE THE APP, AND INSTALL IT

OPEN IT, ADD YOUR SOFTWARE-BROKEN CAMERA, GO TO SYSTEM SETTINGS, AND UPDATE

THE APP WILL SHOW THAT A NEW FIRMWARE IS AVAILABLE, AND IF YOU WANT TO DOWNLOAD IT

(IF NOT, THAT MEAN YOU ENTER THE WRONG IP ADDRESS ON THE FILE, SO CHECK YOUR LOCAL IP ADDRESS)

CLICK YES, BUT THE CAMERA SHOULD NOT UPDATE, BECAUSE SHE'S GOING TO GET THE FILE FROM THE ESCAM FIRMWARE SERVER

## STEP 4 - HACKING YOURSELF

FOR THIS STEP, YOU WILL NEED A DEVICE THAT CAN MAKE MITM ATTACK (REDIRECTION)

PERSONALY, I USE A SMARTPHONE WITH DSPLOIT, BUT IT WILL WORK WITH ANY DEVICE

FIRST, WE NEED TO TARGET THE ESCAM G02

THEN, ON YOUR MITM APP, SET A REDIRECTION FOR ALL TRAFIC FROM THE IP CAMERA, TO THE APACHE SERVER

CLICK ON UPDATE, THE CAMERA WILL GET THE FILE, REBOOT, AND WORK AGAIN

(IF THE CAMERA DOESN'T UPDATE, CONTINUE TO TRY, UNTIL THE MITM REDIRECT AND THE CAMERA GET THE FILE)

YOU MAYBE NEED TO RESET THE CAMERA TO GET ALL THE FUNCTION WORK AGAIN


AND HERE WE GO, YOUR ESCAM G02 WORK GREAT AGAIN !
