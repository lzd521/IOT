---
attachments: [Clipboard_2023-03-06-12-24-19.png, Clipboard_2023-03-06-12-25-04.png, Clipboard_2023-03-06-12-30-02.png, Clipboard_2023-03-06-12-30-04.png, Clipboard_2023-03-06-12-30-56.png, Clipboard_2023-03-06-12-32-23.png, Clipboard_2023-03-06-12-33-05.png, Clipboard_2023-03-06-12-34-08.png, Clipboard_2023-03-06-12-34-47.png]
title: Vul3 TP-Link WPA7510
created: '2023-03-06T04:17:16.110Z'
modified: '2023-03-06T04:34:47.943Z'
---

# Vul3 TP-Link WPA7510
漏洞描述：httpd处理来自/admin/locale的sub_41E970函数时对operation字段未检验传入PRINTF_ECHO函数和excFormatCmd函数造成栈溢出和任意命令执行
![](@attachment/Clipboard_2023-03-06-12-24-19.png)
![](@attachment/Clipboard_2023-03-06-12-25-04.png)
通过form=lang控制v7的值，v7从前端接收operation字段的值并赋给v8。v7不为空且不为read则将v8拼接到PEINTF_ECHO函数
![](@attachment/Clipboard_2023-03-06-12-30-04.png)
该函数直接将a1通过vsprintf函数拼接给v7，并将v7传递给excFormatCmd
![](@attachment/Clipboard_2023-03-06-12-30-56.png)
栈长度为0x800,则opeartion字段长度大于2052即可造成栈溢出
![](@attachment/Clipboard_2023-03-06-12-32-23.png)
excFormatCmd处也会造成栈溢出
![](@attachment/Clipboard_2023-03-06-12-33-05.png)
utils_exec_cmd则会任意命令执行

poc:
![](@attachment/Clipboard_2023-03-06-12-34-08.png)
![](@attachment/Clipboard_2023-03-06-12-34-47.png)

