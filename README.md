# CMD-2ChangeMacAddress
CMD一键修改MAC地址

<center><b><h1>CMD一键修改mac地址</h1></b> </center>

源文件丢失，制作过程中记了日志，供参考。



devcon.exe 修改程序



点击右下角无线网

寻找wifi 勾选自动连接 点击连接成功



chmac.vbs

> 运行chmac.bat不跳黑色窗口

```
createObject("WScript.shell").run "chmac.bat",vbhide
```

 

chmac.bat

> *DEV_095B*为网卡型号，百度如何获取

```bat
 

@echo off

For /f "tokens=3 delims= " %%i in ('Reg Query "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4D36E972-E325-11CE-BFC1-08002BE10318}\0014" /v "NetworkAddress" ') do Set a=%%i

 

rem 内网

if %a% equ 4C34887C907F (goto gt1) else (goto gt2)

:gt1

echo 内网转外网

regedit /s outer.reg

echo 正在禁用本机网卡 

devcon disable *DEV_095B* 

 

echo 正在启用本机网卡 

echo 友情提示：此操作时间较长，请耐心等待，脚本执行完成后，本窗口会自动退出。

devcon enable *DEV_095B*

netsh wlan connect czbank-kyzx

goto n1

:gt2

echo 外网转内网

regedit /s inner.reg

echo 正在禁用本机网卡 

devcon disable *DEV_095B* 

 

echo 正在启用本机网卡 

echo 友情提示：此操作时间较长，请耐心等待，脚本执行完成后，本窗口会自动退出。

devcon enable *DEV_095B*

netsh wlan connect czbank-test

goto n1

:n1 
```

 



以下两文件可用regshot比较编写

outer.reg

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4D36E972-E325-11CE-BFC1-08002BE10318}\0014]

"NetworkAddress"="3A43E5A3D627"
```

 

inner.reg

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4D36E972-E325-11CE-BFC1-08002BE10318}\0014]

"NetworkAddress"="4C34887C907F"
```

 

 

 
