# Nmap

```
nmap -sT -p port-number -O -sC -sV -T[1-5] -oA output-file-name ip-address

#https://github.com/vulnersCom/nmap-vulners
nmap -sV --script vulners [--script-args mincvss=<arg_val>] <target>
nmap --script http-vulners-regex.nse [--script-args paths={"/"}] <target> 

#https://github.com/scipag/vulscan
nmap -sV --script=vulscan/vulscan.nse www.example.com

```
