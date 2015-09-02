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

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce at iaculis ex, quis ultrices tortor. Nunc fringilla est eu commodo pretium. Praesent blandit dui ante, eu viverra est molestie id. Donec at tellus lacinia, sollicitudin nisl et, mattis tellus. Fusce molestie justo nec mauris feugiat tincidunt. Phasellus non dolor tempor, interdum justo ac, tempus justo. Ut porta, nisi quis cursus fringilla, augue massa gravida dui, nec accumsan nisl justo eu quam. Duis sit amet eleifend orci. Nunc ornare nibh commodo lectus ultrices, id luctus sem molestie.

### Shell with Hashes

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce at iaculis ex, quis ultrices tortor. Nunc fringilla est eu commodo pretium. Praesent blandit dui ante, eu viverra est molestie id. Donec at tellus lacinia, sollicitudin nisl et, mattis tellus. Fusce molestie justo nec mauris feugiat tincidunt. Phasellus non dolor tempor, interdum justo ac, tempus justo. Ut porta, nisi quis cursus fringilla, augue massa gravida dui, nec accumsan nisl justo eu quam. Duis sit amet eleifend orci. Nunc ornare nibh commodo lectus ultrices, id luctus sem molestie.

### Shell with Kerberos

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce at iaculis ex, quis ultrices tortor. Nunc fringilla est eu commodo pretium. Praesent blandit dui ante, eu viverra est molestie id. Donec at tellus lacinia, sollicitudin nisl et, mattis tellus. Fusce molestie justo nec mauris feugiat tincidunt. Phasellus non dolor tempor, interdum justo ac, tempus justo. Ut porta, nisi quis cursus fringilla, augue massa gravida dui, nec accumsan nisl justo eu quam. Duis sit amet eleifend orci. Nunc ornare nibh commodo lectus ultrices, id luctus sem molestie.

### Shell with aesKey

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce at iaculis ex, quis ultrices tortor. Nunc fringilla est eu commodo pretium. Praesent blandit dui ante, eu viverra est molestie id. Donec at tellus lacinia, sollicitudin nisl et, mattis tellus. Fusce molestie justo nec mauris feugiat tincidunt. Phasellus non dolor tempor, interdum justo ac, tempus justo. Ut porta, nisi quis cursus fringilla, augue massa gravida dui, nec accumsan nisl justo eu quam. Duis sit amet eleifend orci. Nunc ornare nibh commodo lectus ultrices, id luctus sem molestie.


## Useful combos


### Web Delivery plus WMIEXEC

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce at iaculis ex, quis ultrices tortor. Nunc fringilla est eu commodo pretium. Praesent blandit dui ante, eu viverra est molestie id. Donec at tellus lacinia, sollicitudin nisl et, mattis tellus. Fusce molestie justo nec mauris feugiat tincidunt. Phasellus non dolor tempor, interdum justo ac, tempus justo. Ut porta, nisi quis cursus fringilla, augue massa gravida dui, nec accumsan nisl justo eu quam. Duis sit amet eleifend orci. Nunc ornare nibh commodo lectus ultrices, id luctus sem molestie.

 