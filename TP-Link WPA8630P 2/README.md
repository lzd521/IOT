# Vul2 TP-Link WPA8630P
Vulnerability introduction:
Process the sub of admin/powerline in the httpd file_ There is a command injection vulnerability in the 40A918 function (stack overflow can also be constructed). 
The devicePwd parameter in the function sub_ 40A80C controlled by plc_add is not filtered,but is directly executed by vsprintf splicing, resulting in command injection and stack overflow attack.
Version: TL-WPA8630 KIT (US)_ V2_ Version 171011, and other versions of WPA, WR, WA and other power cat and repeater equipment.


Vulnerability static analysis
![](Clipboard_2023-03-04-21-01-30.png)
When processing web requests from/admin/powerline, run sub_ 40A918 function
![](Clipboard_2023-03-04-21-03-12.png)
Accept the form field from the front end and judge when the form field value is plc_ device execute sub_40A80C
![](Clipboard_2023-03-05-17-36-32.png)
Use this function to process the operation field, and execute sub when the field value is remove sub_40A448
![](Clipboard_2023-03-05-17-43-43.png)
This function processes the value from the key field and directly splices the value into the sub_ 4036A0 function
![](Clipboard_2023-03-04-21-06-10.png)
This function executes system commands
![](Clipboard_2023-03-04-21-06-55.png)
Directly splice the value of a1 to v6 through the insecure vsprintf function
![](Clipboard_2023-03-04-21-08-01.png)
The stack length of v6 is 0x804=2052, stack overflow will occur when the length of a1 exceeds 2052
![](Clipboard_2023-03-05-17-42-52.png)
![](Clipboard_2023-03-04-21-06-55.png)
Directly splice the value of a1 to v6 through the insecure vsprintf function, and pass v6 into the function utils_ exec_ cmd
![](Clipboard_2023-03-04-21-10-39.png)
This function directly assigns a1 to v5, and executes commands through execvp, resulting in arbitrary command execution
![](Clipboard_2023-03-04-21-12-08.png)
getshell
