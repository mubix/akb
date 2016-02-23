# OSX Persistence

Notes from Shakcon 2014 talk:
https://www.youtube.com/watch?v=rhhvZnA4VNY&list=UUIvytJLrQS3x8lTusvTLaOQ

slide deck from synack OSX Persistence
* [local copy](../references/T2_OSXpersistence.pdf)

* `xattr -l dmgfile` - Show if the com.apple.quarantine bit is set

### Unsign apps:

  1. Remove LC_CODE_SIGNATURE block
  2. Modify known good app (add malicious code)
  3. Runs just fine...

### Load un-signed KEXTS

  1. Patch "kextd" deamon in userland to allow: replace `move %eax %ebx` with `xorl %eax %eax`

or

  1. `launchctl unload /System/Library/LaunchDaemons/com.apple.kextd.plist`
  2. `kextload -v unigned.kext` (should get message saying failed to talk to kextd, loading directly into kernel)

### Persistence Locations

#### Kernel Extensions

Note: He mentioned in the talk that you could just do one of the above ways to execute an unsiqned KERNEL extension, but he mentioned that it runs KEXTS at boot (probably earlier than one of the above can happen)

1. Create KEXT (using XCODE)
2. Copy to "KEXT" directory: `/Library/Extensions`
3. Set ownership to 'root': `chown -R root:wheel /Library/Extensions/persistantkext.kext`
4. Rebuild kernel cache: `kextcache -system-prelinked-kernel` and `kextcache -system-caches`

#### Launch Daemons

Create a PLIST file (not complete, needs rest of DOCTYPE line)
```
<?xml version='1.0' encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC ...>
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>com.example.persist</string>
	<key>ProgramArguments</key>
	<array>
		<string>/path/to/persistbin</string>
		<string>args?</string>
	</array>
	<key>RunAtLoad</key>
	<true/>
</dict>
</plist>
```

#### Cronjobs

1. `echo "* * * * * echo \"I'm persisting\"" > /tmp/persistJob`
2. `crontab /tmp/persistJob`

#### Login or Logout Hook

1. `defaults write com.apple.loginwindow LoginHook /usr/bin/hook.sh`

#### Login Items

Warning: This is visible in the Login Items GUI (System Preferences -> User & Groups -> Login Items)

1. PLIST file: `~/Library/Preferences/com.apple.loginitems.plist`

or

1. Copy the app to persist into `<main>.app/contents/library/loginitems`
2. In "main".app invoke SMLOGINITEMSETENABLED()
  ```
  //enable auto launch
  SMLoginItemSetEnabled((__bridge CFStringRef)@"com.company.persistMe", YES);
  ```

`/private/var/db/launch.db/ -> com.apple.alaunchd.peruser.501/overrides.plist` is mentioned, in the slides, but I'm not sure why

#### Reopend Apps

Apps that are said to have been open during the last logout and will be reopened the next time the user logs in

Stored in `~/Libraries/Preferences/ByHost/com.apple.loginwindow.SOMEHARDWAREUUIDHERE.plist`

#### Startup Items

1. Create script, below is suggested example:
  ```
  #!/bin/sh
  . /etc/rc.common

  StartService()
  {
    #anything here

  }

  RunService "$1"
  ```
2. Create "StartupParameters.plist"
  ```
  {
  	Description = "anything here";
  	Providers = ("name of file");
  }
  ```
3. Place both in `/System/Library/StartupItems` or `/Library/StartupItems`, in it's own folder. Name of folder has to match the name of the script (case sensitive)

#### RC.COMMON

1. Add anything to `rc.common`

#### LaunchD.conf

File isn't there by default. If you create it, it's loaded by LaunchD at startup

1. Example:
  ```
  echo bsexec 1 /bin/bash evil.sh > /etc/launchd.conf
  ```

"bsexec" is a Launchd command that executes commands

#### Application Specific (plugins or extensions that are app specific)

1. Use a "constructor" so that even if the App doesn't load a specific item that it's looking for, if the app loads the file, it runs. For example:
  ```
  #includ <syslog.h>
  //automatically invoked when loaded
  __attribute__((constructor)) void myConstructor()
  {
  	// dbg msg
  	syslog(LOG_ERR, "loaded in process %d", getpid());
  	return;
  }
  ```

### No longer in Mavericks

1. `~/.MacOSX/environment.plist`
2. `/Library/Preferences/com.apple.SystemLoginItems.plist`
3. `loginwindows 'AutoLaunchedApplicationsDictionary'`
4. "DYLD_INSERT_LIBRARIES" (signed)

