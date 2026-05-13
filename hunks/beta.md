# beta

**offset:** 458ba4
**patches**: compatiblity tester
*"The compatibility tester now defaults to romance ratings regardless of miis' gender."*

```
length: 0x34
cmp r5, r9
movne r6, #3
bne 0x20
cmp r5, #0
moveq r6, #2
beq 0x20
mov r6, #1
b 0x20
nop 
nop 
nop 
mov r5, #1
b 0x54
```
