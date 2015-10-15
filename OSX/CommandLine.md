Cool way to sniff traffic

Source: https://twitter.com/fel1x/status/613420320104558592
```
sudo dtrace -n 'pid$target::SecKeychainLogin:entry{trace(copyinstr(uregs[R_ECX]));}' -p $(ps -A | grep -m1 loginwindow | awk '{print $1}') 
``
