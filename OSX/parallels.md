# Parallels Desktop


## Get memory dump from VM

Source: http://kb.parallels.com/en/121323

`prlctl` binary is in `/usr/local/bin`

1. List any VMs
```
prlctl list -a
```

1a. Start if needed
```
prlctl start {5121-a2134-adfsdf} <- whatever the UUID is of the one listed from the last command
```

2. Run the dpgdump command (dumping memory.dmp to Desktop of current user)
```
prlctl internal {5121-a2134-adfsdf} dbgdump --path ~/Desktop/
```

Once this is done, Volatility and Rekal work just fine against the dump.

