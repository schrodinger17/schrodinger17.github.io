出题人给hint说不要用IDA，但我在这之前已经用IDA做完了……

其实只需要调整一下堆栈平衡，patch一些花指令，这题并没有什么难度，但是这么些个逻辑运算真的恶心

首先从main函数走到了`sub_401E10`，所有的操作都在这里处理

```c++
  length0 = strlen(commandline_input);
  input0[0] = 0;
  memset(&input0[1], 0, 0x3FCu);
  v51 = 0;
  v52 = 0;
  scanf("%s", input0);
  length1 = strlen(input0);
  length_input = length1;
```

`commandline_input`可以不管它，先是输入了flag然后获取了长度，然后走第一个判断

```c++
      input0[length_input] = 10;
      v48 = 32;
      for ( i = 0; i < length_input; ++i )
        input0[i] -= v48;
      v45 = (unsigned __int8)input0[2] > length_input;
      v44 = (unsigned __int8)input0[1] > v48;
      if ( alwaystrue12(v44, v45) == 1 )
        sub_40105A();                           // 第一步
      v45 = (unsigned __int8)input0[2] > length_input;
      v44 = (unsigned __int8)input0[1] > v48;
      if ( !alwaystrue12(v44, v45) )
        sub_401064();
```

用到的判断是一个永真判断，所以默认执行第一个函数，初始化`target1`为奇数

```c++
int sub_401870()
{
  int result; // eax
  signed int i; // [esp+50h] [ebp-4h]

  for ( i = 0; i <= 5; ++i )
  {
    target1[i] = 2 * i + 1;
    result = i + 1;
  }
  return result;
}
```

下面的几个逻辑判断发现也是永真，执行到这一步

```c++
if ( alwaystrue1(SLODWORD(v46), SHIDWORD(v46)) == 1 )
          deal_with_4321c0();
```

初始化了一个`tttt`的数组，然后又执行了一个函数

```c++
int (*sub_401A10())()
{
  tttt[0] = pow(2, 2) + 1;
  dword_4321C4 = tri(1);
  dword_4321C8 = 1;
  dword_4321CC = tri(1);
  dword_4321D0 = pow(2, 2) + 2;
  dword_4321D4 = 1;
  dword_4321D8 = 1;
  return off_42EA3C[0];
}
```

但是这里的处理显然是假的，后面返回的函数又对这个数组进行了重新的赋值，真正的赋值部分在下面

```c++
void *sub_4015F0()
{
  void *retaddr; // [esp+0h] [ebp+0h]

  tttt[0] = pow(2, 2);
  dword_4321C4 = pow(2, 2) + 1;
  dword_4321C8 = 2;
  dword_4321CC = tri(1);
  dword_4321D0 = 1;
  dword_4321D4 = pow(2, 2);
  dword_4321D8 = pow(2, 1);
  return retaddr;
}
```

然后又是一个永真的判断

```c++
		v1 = sqrt(v46);
        v2 = _ftol(v1);
        v3 = pow(5, v2) - 1;
        if ( !(v3 % pow(2, 2)) )                // 第三步
        {
          for ( i = 0; i < length_input; ++i )
            input0[i] += input0[i + 1];
          for ( i = length_input - 1; i > 0; --i )
            input0[i] += *((_BYTE *)&length_input + i + 3);
          for ( i = length_input - 1; i > 0; --i )
            byte_4323C0[i] += v48 + *((_BYTE *)&length_input + i + 3);
          hex2str((int)input0, (int)input_step2, length_input);
          sub_401073();
        }
```

输入进行了几步处理，然后把输入的字符16进制转成字符串存储起来，长度翻倍，最后执行了`sub_401073()`函数，跳到了这个位置

```c++
int (*sub_4017A0())()
{
  signed int i; // [esp+50h] [ebp-4h]

  for ( i = 0; i <= 64; ++i )
    final_target[i] = i * i;
  return off_42EA40;
}
```

`return`了一个偏移量，~~发现事情并不单纯~~，应该和上面一样又套了一层

```c++
void *__usercall sub_401680@<eax>(int a1@<ebp>)
{
  void *retaddr; // [esp+0h] [ebp+0h]

  memset(dword_4328C0, 0, 0x400u);
  memset(final_target, 0, 0x400u);
  for ( *(_DWORD *)(a1 - 8) = 2; *(_DWORD *)(a1 - 8) <= dword_42EA30; ++*(_DWORD *)(a1 - 8) )
  {
    if ( !dword_4328C0[*(_DWORD *)(a1 - 8)] )
      final_target[++final_target[0]] = *(_DWORD *)(a1 - 8);
    for ( *(_DWORD *)(a1 - 4) = 1; *(_DWORD *)(a1 - 4) <= final_target[0]; ++*(_DWORD *)(a1 - 4) )
    {
      if ( final_target[*(_DWORD *)(a1 - 4)] * *(_DWORD *)(a1 - 8) > dword_42EA30 )
        break;
      dword_4328C0[final_target[*(_DWORD *)(a1 - 4)] * *(_DWORD *)(a1 - 8)] = 1;
      if ( !(*(_DWORD *)(a1 - 8) % final_target[*(_DWORD *)(a1 - 4)]) )
        break;
    }
  }
  return retaddr;
}
```

处理看起来不是很简单，但是和输入没什么关系，直接动态调试`dump`出来就行了，没什么难度

接着进行了一个逆序

```c++
reverse((int)input_step2, 2 * length_input);// 第四步
```

接下来的几个逻辑判断判断一下就不难发现最后走的是这一部分的处理

```c++
        else                                    // 第五步
        {
          for ( i = 0; i < 2 * length_input; ++i )
          {
            v14 = tri(1);
            v15 = pow(2, v14);
            v16 = i;
            input_step2[i] += LOBYTE(tttt[i % (v15 - 1)]);
            HIDWORD(v46) = i;
            LOBYTE(v16) = input_step2[i];
            sub_401019(v16, i);
            input_step2[i] = v17;
            *(&str2 + i) = (unsigned __int8)input_step2[i] + 2 * i;
          }
        }
```

`sub_401019(v16, i)`的内容看一下也很容易出来

```c++
int __cdecl sub_401CD0(char a1, int a2)
{
  signed int v2; // ST58_4

  v2 = sub_40101E(4) - 2;
  return a2 % (signed int)(sub_40101E(3) - 2) ^ target1[a2 % v2] ^ a1;
}
```

简单的两层异或，中间套了一个函数，传的是固定参数，调试很容易出来，不过这个函数也不复杂，可以直接分析出来

```c++
size_t __cdecl sub_401820(int a1)
{
  return a1 * strlen(off_42EA34);
}
```

求得是这一部分的长度

```asm
.data:0042EA34     ; char *off_42EA34
.data:0042EA34     off_42EA34      dd offset unk_42C01C    ; DATA XREF: sub_401820+18↑r
.data:0042EA38     off_42EA38      dd offset sub_401023    ; DATA XREF: sub_401950+7C↑r
.data:0042EA3C     off_42EA3C      dd offset sub_401050    ; DATA XREF: sub_401A10+7F↑r
.data:0042EA40     off_42EA40      dd offset sub_40103C    ; DATA XREF: sub_4017A0+46↑r
.data:0042EA44                     align 10h
```

但第一部分是数据，肯定不是整个的长度，一定有`\0`让`strlen`截断

```c++
.rdata:0042C01C     unk_42C01C      db  58h ; X             ; DATA XREF: .data:off_42EA34↓o
.rdata:0042C01D                     db  58h ; X
.rdata:0042C01E                     db    0
.rdata:0042C01F                     db    0
```

发现长度为2，并且在`.rdata`段，程序运行过程中没有修改，检验一下查一查交叉引用发现果然没有，回到原来的位置，`sub_40101E`的作用很简单，就是`*2`，所以返回的值也可以写成

```c++
return a2 % 4 ^ target1[a2 % 6] ^ a1
```

下面再稍微判断一下，永远走的是这个处理

```c++
        if ( v25 >= pow(v40 / 2, 2) )           // 第六步
        {
          for ( i = 0; i < 2 * length_input; ++i )
          {
            v26 = tri(1);
            v27 = pow(2, v26);
            *(&str2 + i) *= final_target[i % (v27 - 1)];
            v28 = pow(i, 2);
            *(&str2 + i) += v28 - i;
          }
        }
```

然后四位变五位

```c++
        for ( i = 0; i < 2 * length_input; ++i )// 第七步
        {
          memset(&str1, 0, 0x16u);
          tmp = *(&str2 + i);
          sprintf(&str1, "%05d", tmp);
          strcat(&str0, &str1);
        }
```

几步处理之后继续16进制转字符串

```c++
        length2 = strlen(&str0);
        for ( i = 0; i < length2; ++i )
        {
          v33 = tri(1);
          v34 = pow(2, v33);
          v35 = i % (v34 - 1);
          *(&str0 + i) += i % pow(2, 2) + tttt[v35];
        }
        reverse((int)&str0, length2);
        hex2str((int)&str0, (int)&hex, length2);
```

最后进行比较

```c++
if ( !strcmp(
                &hex,
                "3D393A37343C39373A343A373D36363A3B3934333539363437373934383736373B38333D3D3D37313C3B3A36353C393437373C38"
                "3E343434393B34343C37343E373B3C3938343C3C39383238373F36323C3C3933383939363535373A373535393F373D34373C3435"
                "35393C39383336363B37333639353435393B3D313636363B35363833383D35333E3A3532383837353438373E373437343E34363A"
                "3D3A3233393939323735393A3D363B3B3736333B3C3436313F3C3D3435363537353739343A33343239383334363E3339373B373A"
                "3634373D3B3632373C3B35323D373732383739353435353A3834353538343934") )
          printf("PASS!\n");
        else
          printf("WHAT?\n");
```

整个过程没有什么特别复杂的处理，逆起来也没什么难度

```python
finaltarget = [54, 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101,
               103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211,
               223, 227, 229, 233, 239, 241, 251]
tttt = [4, 5, 2, 3, 1, 4, 2]
target1 = [1, 3, 5, 7, 9, 11]
final = "3D393A37343C39373A343A373D36363A3B3934333539363437373934383736373B38333D3D3D37313C3B3A36353C393437373C383E343434393B34343C37343E373B3C3938343C3C39383238373F36323C3C3933383939363535373A373535393F373D34373C343535393C39383336363B37333639353435393B3D313636363B35363833383D35333E3A3532383837353438373E373437343E34363A3D3A3233393939323735393A3D363B3B3736333B3C3436313F3C3D3435363537353739343A33343239383334363E3339373B373A3634373D3B3632373C3B35323D373732383739353435353A3834353538343934"
f = []
i = 0
while i < len(final):
    f.append(int(final[i:i + 2], 16))
    i += 2
f.reverse()
for i in range(len(f)):
    f[i] -= i % 4 + tttt[i % 7]
# print(f)
tmp = ''.join([chr(i) for i in f])
# print(tmp)
str2 = []
j = 0
while j < len(tmp):
    str2.append(int(tmp[j:j + 5]))
    j += 5
# print(tmp_f)
for i in range(len(str2)):
    str2[i] -= (i ** 2 - i)
    str2[i] //= finaltarget[i % 7]
# print(str2)
input_step2 = []
for i in range(len(str2)):
    input_step2.append((((str2[i] - 2 * i) ^ (i % 4) ^ (target1[i % 6])) & 0xff) - tttt[i % 7] & 0xff)
input_step2.reverse()
input0 = ''.join([chr(i) for i in input_step2])
inp = []
i = 0
while i < len(input0):
    inp.append(int(input0[i:i + 2], 16))
    i += 2

for i in range(1, len(inp)):
    inp[i] -= inp[i - 1]
    inp[i] &= 0xff
# print([hex(i) for i in inp])
inp.append(10)
i = len(inp) - 1
while i > 0:
    inp[i - 1] -= inp[i]
    i -= 1
    inp[i] &= 0xff

for i in range(len(inp) - 1):
    inp[i] += 32
flag = ''.join([chr(i) for i in inp])
print(flag)
```

最后的flag

```
NNPPUUCTF{S0_M4NY_BUG5!}
```

