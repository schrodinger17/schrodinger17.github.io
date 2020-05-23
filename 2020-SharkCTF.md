### 2020-SharkCTF

#### rev

##### easyre

An easy asm, as we can see, every single `byte` in `the_second_array` is xored, and then plus the `byte` with the same index in `some_array`

```asm
BITS 64

SECTION .rodata
	some_array db 10,2,30,15,3,7,4,2,1,24,5,11,24,4,14,13,5,6,19,20,23,9,10,2,30,15,3,7,4,2,1,24
	the_second_array db 0x57,0x40,0xa3,0x78,0x7d,0x67,0x55,0x40,0x1e,0xae,0x5b,0x11,0x5d,0x40,0xaa,0x17,0x58,0x4f,0x7e,0x4d,0x4e,0x42,0x5d,0x51,0x57,0x5f,0x5f,0x12,0x1d,0x5a,0x4f,0xbf
	len_second_array equ $ - the_second_array
SECTION .text
    GLOBAL main

main:
	mov rdx, [rsp]
	cmp rdx, 2
	jne exit
	mov rsi, [rsp+0x10]
	mov rdx, rsi
	mov rcx, 0
l1:
	cmp byte [rdx], 0
	je follow_the_label
	inc rcx
	inc rdx
	jmp l1
follow_the_label:
	mov al, byte [rsi+rcx-1]
	mov rdi,  some_array
	mov rdi, [rdi+rcx-1]
	add al, dil
	xor rax, 42
	mov r10, the_second_array
	add r10, rcx
	dec r10
	cmp al, byte [r10]
	jne exit
	dec rcx
	cmp rcx, 0
	jne follow_the_label
win:
	mov rdi, 1
	mov rax, 60
	syscall
exit:
	mov rdi, 0
	mov rax, 60
	syscall
```

So it's easy to solve it with python using simple script blow

```python
target=[0x57,0x40,0xa3,0x78,0x7d,0x67,0x55,0x40,0x1e,0xae,0x5b,0x11,0x5d,0x40,0xaa,0x17,0x58,0x4f,0x7e,0x4d,0x4e,0x42,0x5d,0x51,0x57,0x5f,0x5f,0x12,0x1d,0x5a,0x4f,0xbf]
some_array=[10,2,30,15,3,7,4,2,1,24,5,11,24,4,14,13,5,6,19,20,23,9,10,2,30,15,3,7,4,2,1,24]
flag=""
for i,t in enumerate(target):
    flag+=chr((t^42)-some_array[i])
print(flag)

# shkCTF{h3ll0_fr0m_ASM_my_fr13nd}
```

##### z3_robot

Just input and then pass a few equations, so it's quite easy to solve it with z3 as the title says

```python
from z3 import *

a1 = []
for i in range(25):
    a1.append(BitVec("s%d" % i, 32))

solver = Solver()

solver.add((a1[20] ^ 0x2B) == a1[7])
solver.add(a1[21] - a1[3] == -20)
solver.add((a1[2] >> 6) == 0)
solver.add(a1[13] == 116)
solver.add(4 * a1[11] == 380)
solver.add(a1[7] >> a1[17] % 8 == 5)
solver.add((a1[6] ^ 0x53) == a1[14])
solver.add(a1[8] == 122)
solver.add(a1[5] << a1[9] % 8 == 392)
solver.add(a1[16] - a1[7] == 20)
solver.add(a1[7] << a1[23] % 8 == 190)
solver.add(a1[2] - a1[7] == -43)
solver.add(a1[21] == 95)
solver.add((a1[2] ^ 0x47) == a1[3])
solver.add(a1[0] == 99)
solver.add(a1[13] == 116)
solver.add((a1[20] & 0x45) == 68)
solver.add((a1[8] & 0x15) == 16)
solver.add(a1[12] == 95)
solver.add(a1[4] >> 4 == 7)
solver.add(a1[13] == 116)
solver.add(a1[0] >> a1[0] % 8 == 12)
solver.add(a1[10] == 95)
solver.add((a1[8] & 0xAC) == 40)
solver.add(a1[16] == 115)
solver.add((a1[22] & 0x1D) == 24)
solver.add(a1[9] == 51)
solver.add(a1[5] == 49)
solver.add(4 * a1[19] == 456)
solver.add(a1[20] >> 6 == 1)
solver.add(a1[7] >> 1 == 47)
solver.add(a1[1] == 108)
solver.add(a1[3] >> 4 == 7)
solver.add((a1[19] & 0x49) == 64)
solver.add(a1[4] == 115)
solver.add((a1[2] & a1[11]) == 20)
solver.add(a1[0] == 99)
solver.add(a1[4] + a1[5] == 164)
solver.add(a1[15] << 6 == 6080)
solver.add((a1[10] ^ 0x2B) == a1[17])
solver.add((a1[12] ^ 0x2C) == a1[4])
solver.add(a1[19] - a1[21] == 19)
solver.add(a1[12] == 95)
solver.add(a1[15] >> 1 == 47)
solver.add(a1[19] == 114)
solver.add(a1[17] + a1[18] == 168)
solver.add(a1[22] == 58)
solver.add((a1[23] & a1[21]) == 9)
solver.add(a1[6] << a1[19] % 8 == 396)
solver.add(a1[3] + a1[7] == 210)
solver.add((a1[22] & 0xED) == 40)
solver.add((a1[12] & 0xAC) == 12)
solver.add((a1[18] ^ 0x6B) == a1[15])
solver.add((a1[16] & 0x7A) == 114)
solver.add((a1[0] & 0x39) == 33)
solver.add((a1[6] ^ 0x3C) == a1[21])
solver.add(a1[20] == 116)
solver.add(a1[19] == 114)
solver.add(a1[12] == 95)
solver.add(a1[2] == 52)
solver.add(a1[23] == 41)
solver.add(a1[10] == 95)
solver.add((a1[22] & a1[9]) == 50)
solver.add(a1[3] + a1[2] == 167)
solver.add(a1[17] - a1[14] == 68)
solver.add(a1[21] == 95)
solver.add((a1[19] ^ 0x2D) == a1[10])
solver.add(4 * a1[12] == 380)
solver.add(a1[6] & 0x40 != 0)
solver.add((a1[12] & a1[22]) == 26)
solver.add(a1[7] << a1[19] % 8 == 380)
solver.add((a1[20] ^ 0x4E) == a1[22])
solver.add(a1[6] == 99)
solver.add(a1[12] == a1[7])
solver.add(a1[19] - a1[13] == -2)
solver.add(a1[14] >> 4 == 3)
solver.add((a1[12] & 0x38) == 24)
solver.add(a1[8] << a1[10] % 8 == 15616)
solver.add(a1[20] == 116)
solver.add(a1[6] >> a1[22] % 8 == 24)
solver.add(a1[22] - a1[5] == 9)
solver.add(a1[7] << a1[22] % 8 == 380)
solver.add(a1[22] == 58)
solver.add(a1[16] == 115)
solver.add((a1[23] ^ 0x1D) == a1[18])
solver.add(a1[23] + a1[14] == 89)
solver.add((a1[5] & a1[2]) == 48)
solver.add((a1[15] & 0x9F) == 31)
solver.add(a1[4] == 115)
solver.add((a1[23] ^ 0x4A) == a1[0])
solver.add((a1[6] ^ 0x3C) == a1[11])
flag = ""
if solver.check() == sat:
    r = solver.model()
    for i in range(24):
        flag += chr(r[a1[i]].as_long() & 0xff)
print(flag)
# shkCTF{cl4ss1c_z3___t0_st4rt_:)}
```

##### secure_db

I can't see anything using IDA so I debugged it and I found the input got processed and then compared with `N3kviX7-vXEqvlp` , I just input this string and got an interesting string `T4h7s_4ll_F0lks` which seems like to be the right one.

So this reversing algorithm seems to be itself, and I tried to rewrite it with python and I got the same answer

```python
target = "N3kviX7-vXEqvlp"
x = [0x1a, 0x07, 0x03, 0x41]
key = ""
for i, c in enumerate(target):
    key += chr(ord(c) ^ x[i % 4])
print(key)

# T4h7s_4ll_F0lks
```

Just input the key and we will get a database file from the server, and there is the flag.

##### miss_direction

Actually I haven't solved it yet, I however managed to anti anti-debug (just remove tls table from PE headers will be fine), and then the exception handler really confused me,I can't find anything to do with my input, I really got missed... 