# Registry

```
# Поиск в значениях и ключах фразы password
reg query HKLM /f password /t REG_SZ /s
# Вывод значения реестра
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"
```
