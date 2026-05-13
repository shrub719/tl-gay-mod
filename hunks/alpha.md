# alpha

**offset:** 232a34
**patches:** housing
*"Stable housing for gay miis."*

- nopping 0x232a34 makes absolutely no housing appear
- it's branched to during startup

```
ldr r1, [r9, #0x15c]
ldrb r1, [r1, #0x639]
add r1, r5, r1, lsl #1
add r1, r1, #0x700
ldrsh r2, [r1, #0xe]
cmn r2, #1
```

