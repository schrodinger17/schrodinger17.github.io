re选手做完两道题结束比赛……

#### guessgame

随机数用时间种子初始化过了，猜是猜不对的，而且猜对了也没用，会输出flag不在这里，但是可以直接用IDA直接找到flag……

```
BJD{S1mple_ReV3r5e_W1th_0D_0r_IDA}
```

#### reverse-8086_ASM-DreamerJack

16位DOS……

IDA打开，发现一段数据很奇怪，转换成code

```asm
mov     cx, 22h ; '"'
seg001:0005                 lea     bx, aUDuTZWjQGjzZWz ; "]U[du~|t@{z@wj.}.~q@gjz{z@wzqW~/b;"
seg001:0009
seg001:0009 loc_10039:                              ; CODE XREF: seg001:000F↓j
seg001:0009                 mov     di, cx
seg001:000B                 dec     di
seg001:000C                 xor     byte ptr [bx+di], 1Fh
seg001:000F                 loop    loc_10039
seg001:0011                 lea     dx, aUDuTZWjQGjzZWz ; "]U[du~|t@{z@wj.}.~q@gjz{z@wzqW~/b;"
seg001:0015                 mov     ah, 9
seg001:0017                 int     21h             ; DOS - PRINT STRING
seg001:0017                                         ; DS:DX -> string terminated by "$"
seg001:0019                 retn
```

异或，循环，输出……

跑一下

```python
target = [0x5D, 0x55, 0x5B, 0x64, 0x75, 0x7E, 0x7C, 0x74, 0x40, 0x7B, 0x7A, 0x40, 0x77, 0x6A, 0x2E, 0x7D, 0x2E, 0x7E,
          0x71, 0x40, 0x67, 0x6A, 0x7A, 0x7B, 0x7A, 0x40, 0x77, 0x7A, 0x71, 0x57, 0x7E, 0x2F, 0x62, 0x3B]
flag = ''
for i in range(0x22):
    flag += chr(target[i] ^ 0x1F)
print(flag)
```

输出flag，其中**`$`是DOS终止符**

```
BJD{jack_de_hu1b1an_xuede_henHa0}$
```

