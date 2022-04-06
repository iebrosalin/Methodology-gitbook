---
description: >-
  Если значение в реестре имеет 1 в политике, то позволяет запускать msi от
  имени админа
---

# AlwaysInstallElevated



```
HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Installer

#Проверить значение
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```
