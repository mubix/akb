# Misculaneous Bash commands:

## Check Hosts
If you have a key or password that you want to check:
(Warning: noisy)

```
for i in {0..254}; do \
  host="192.168.0.$i"; \
  if ssh "root@${host}" "echo -n"; then echo $host; fi \
done
```

