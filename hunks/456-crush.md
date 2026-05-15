# 456xxx crush

**patches:** FUN_00556104  
**purpose:** deciding crushes (i think)  
*"All miis are now bisexual: love events including **confessions**, rivalries, marriage, and childbirth are no longer gender restricted."*

- opened the game several times to only black/orange windows, no freeze
- froze on screenshots below (is that a pixel of pink or is it just me?)
- does not freeze on pink windows that are about proposals (i.e. relationships that already exist)
- when offset 0x4561ee (the first hunk), it only freezes on some pink confession windows,
  but when offset 0x456104 (the start of the function), all pink confession windows
  cause freezes
- sometimes it freezes on the white flash, sometimes on the island preview - is graphics async
  to window logic?

![](./assets/456-1.png)  
![](./assets/456-2.png)  
![](./assets/456-3.png)  

## decomp

- r1 is a boolean that stores... something
- patch still sets r1 even though it doesn't branch depending on its value
    because it gets stored to [sp,#local_3c] right after (that's all though)
- `FUN_00539594`
    - loads what's at the address pointed to by `param_1`, offset by `0x1e4cao`,
        then adds `param_2 * 0x160`
    - i guess `param_1` is an `int **` then?
    - `0x1e4ca0` is a huge ass offset from where `param_1` is pointing.. what could
        possibly be there
- ^ these notes are mostly garbage the majority of the work happens in ghidra lol, but enjoy

```
// patched
  61ec ldr  r0,[r9,#0x0]=>local_38
  61f0 ldr  r1,[r9,#local_34]
  61f4 cmp  r0,r1
  61f8 movlt r1,#0x0
  61fc movge r1,#0x1
  6200 nop
  6204 cpy  r2,r4
  6208 nop
  620c ldr  r0,[r7,#0x0]=>DAT_00acb60c
  6210 nop
  6214 nop

// unpatched
  61ec ldr  r0,[r8,#0x0]=>DAT_00acb5c0
  61f0 cpy  r1,r4
  61f4 add  r0,r0,#0x1e4000
  61f8 add  r0,r0,#0xdc0
  61fc bl   FUN_00539594
  6200 ldr  r0,[r0,#0xec]
  6204 cpy  r2,r4
  6208 cmp  r0,#0x0
  620c ldr  r0,[r7,#0x0]=>DAT_00acb60c
  6210 movne r1,#0x1
  6214 moveq r1,#0x0


int FUN_00539594(int *param_1,int param_2)
{
  return *(int *)(*param_1 + 0x1e4ca0) + param_2 * 0x160;
}
```

## hunks

```
offset: 0x4561EE
length: 0x2A

offset: 0x4567FC
length: 0x4

offset: 0x456A23
length: 0x1

offset: 0x456AC8
length: 0x4
```
