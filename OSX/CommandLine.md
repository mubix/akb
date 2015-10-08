Cool way to sniff traffic
```
sudo dtrace -n 'pid$target::SecKeychainLogin:entry{trace(copyinstr(uregs[R_ECX]));}' -p $(ps -A | grep -m1 loginwindow | awk '{print $1}') (from https://twitter.com/fel1x/status/613420320104558592)
``
