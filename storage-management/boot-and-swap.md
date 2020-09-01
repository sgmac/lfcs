# Boot and swap

- Configuration in `/etc/fstab`. Use UUID
- Test `mount -a`
- `mkswap`
- `swapon| swapoff -a`

- Use `vmstat` to monitor swap

## Memory

- **Active(anon)** memory used more recently and usually not swapped out.
- **Inactive(anon)** memory that can be swapped out.
- **Unevictable** pages that can't be swapped out.
- **Mlocked** pages locked to memoery using `mlock()` system call.


The `Active/Inactive(file)` applies the same but for `pagecache` memory.

```
grep -i active /proc/meminfo                                                                                  ⏎ [~]
Active:         12295132 kB
Inactive:       33294080 kB
Active(anon):    8045644 kB
Inactive(anon):   766032 kB
Active(file):    4249488 kB
Inactive(file): 32528048 kB
```

