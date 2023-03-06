# Vul3 TP-Link WPA7510
Vulnerability description: httpd handles the sub_41E970 function from /admin/locale without checking the operation field passed into the PRINTF_ECHO function and the excFormatCmd function, resulting in a stack overflow and arbitrary command execution.
![](Clipboard_2023-03-06-12-24-19.png)
![](Clipboard_2023-03-06-12-25-04.png)
The value of v7 is controlled via form=lang. v7 receives the value of the operation field from the front end and assigns it to v8. v7 is not empty and is not read then v8 is spliced into the PEINTF_ECHO function
![](Clipboard_2023-03-06-12-30-04.png)
This function stitches a1 directly to v7 via the vsprintf function and passes v7 to excFormatCmd
![](Clipboard_2023-03-06-12-30-56.png)
If the stack length is 0x800, a stack overflow can be caused by the length of the option field being greater than 2052.
![](Clipboard_2023-03-06-12-32-23.png)
The stack overflow is also caused at excFormatCmd
![](Clipboard_2023-03-06-12-33-05.png)
utils_exec_cmd will execute any command

poc:
![](@Clipboard_2023-03-06-12-34-08.png)
![](@Clipboard_2023-03-06-12-34-47.png)
