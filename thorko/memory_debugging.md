# memory debugging

get memory tables

```bash
cat /proc/PID/smaps > /tmp/before.mem
grep "^Size" /tmp/before.mem
```

after memory increases

```bash
cat /proc/PID/smaps > /tmp/after.mem
grep "^Size" /tmp/after.mem
```

 dump addresses

```bash
gdb -p PID
dump memory file 0xc092a00000 0xc099c00000
# or entire core dump
gcore PID
# core.PID will be created
```

