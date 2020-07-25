### DASCTF-July

很久没打比赛了，找了道re做了做

#### simple

打开看到一个tea算法不过是假的，得到了一个假的flag，调试之后发现真正的加密过程

```cpp
void __fastcall __noreturn sub_1388B20(__int64 a1, unsigned __int64 *a2, __int64 a3, __int64 a4)
{
  __int64 v4; // rdx
  __int64 v5; // rcx
  signed int i; // [rsp+8h] [rbp-58h]
  signed int j; // [rsp+Ch] [rbp-54h]
  signed int l; // [rsp+10h] [rbp-50h]
  signed int k; // [rsp+14h] [rbp-4Ch]
  unsigned __int64 v10; // [rsp+20h] [rbp-40h]
  unsigned __int64 v11; // [rsp+28h] [rbp-38h]
  __int64 v12; // [rsp+30h] [rbp-30h]
  char v13; // [rsp+41h] [rbp-1Fh]
  char v14; // [rsp+42h] [rbp-1Eh]
  char v15; // [rsp+43h] [rbp-1Dh]
  char v16; // [rsp+44h] [rbp-1Ch]
  char v17; // [rsp+45h] [rbp-1Bh]
  char v18; // [rsp+46h] [rbp-1Ah]
  char v19; // [rsp+47h] [rbp-19h]
  char v20; // [rsp+48h] [rbp-18h]
  char v21; // [rsp+49h] [rbp-17h]
  char v22; // [rsp+4Ah] [rbp-16h]
  char v23; // [rsp+4Bh] [rbp-15h]
  char v24; // [rsp+4Ch] [rbp-14h]
  char v25; // [rsp+4Dh] [rbp-13h]
  char v26; // [rsp+4Eh] [rbp-12h]
  char v27; // [rsp+4Fh] [rbp-11h]
  char v28; // [rsp+50h] [rbp-10h]
  char v29; // [rsp+51h] [rbp-Fh]
  char v30; // [rsp+52h] [rbp-Eh]
  char v31; // [rsp+53h] [rbp-Dh]
  char v32; // [rsp+54h] [rbp-Ch]
  char v33; // [rsp+55h] [rbp-Bh]
  char v34; // [rsp+56h] [rbp-Ah]
  char v35; // [rsp+57h] [rbp-9h]
  unsigned __int64 v36; // [rsp+58h] [rbp-8h]

  v36 = __readfsqword(0x28u);
  v23 = 0x84u;
  v24 = 0xB2u;
  v25 = 0xA8u;
  v26 = 0xAFu;
  v27 = 0xFDu;
  v28 = 0xB4u;
  v29 = 0xB3u;
  v30 = 0xADu;
  v31 = 0xA8u;
  v32 = 0xA9u;
  v33 = 0xFDu;
  v34 = 0xE7u;
  v35 = 0xFDu;
  v13 = 0x9Cu;
  v14 = 0x87u;
  v15 = 0x89u;
  v16 = 0x86u;
  v17 = 0x9Au;
  v18 = 0x88u;
  v19 = 0x8Du;
  v20 = 0x90u;
  v21 = 0x91u;
  v22 = 0x98u;
  for ( i = 0; i <= 12; ++i )
    sub_412040((unsigned __int8)(*(&v23 + i) ^ 0xDD), (__int64)a2, a3, a4);
  sub_411E40((signed __int64)a2, (signed __int64)a2);
  v10 = *a2;
  v11 = a2[1];
  v12 = 0LL;
  for ( j = 0; j <= 31; ++j )
  {
    v10 += v12 ^ (((v11 >> 5) ^ 16 * v11) + v11);
    v12 -= 7046029253867505223LL;
    v4 = (v10 >> 5) ^ 16 * v10;
    v11 += v12 ^ (v4 + v10);
  }
  if ( v10 != -5959471385066171734LL || v11 != 6225660574651893449LL )
  {
    for ( k = 0; k <= 4; ++k )
      sub_412040((unsigned __int8)~*(&v18 + k), (__int64)a2, v4, v5);
    sub_412040(0xAu, (__int64)a2, v4, v5);
  }
  else
  {
    for ( l = 0; l <= 4; ++l )
      sub_412040((unsigned __int8)(*(&v13 + l) ^ 0xEE), (__int64)a2, v4, v5);
    sub_412040(0xAu, (__int64)a2, v4, v5);
  }
  sub_4104D0(0LL);
}
```

第一个循环用于输出`Your imput :`，然后是一个变种的xTea算法，`key`均为0，所有的数据都是64位，然后判断结果输出`right`或者`wrong`，所以直接写脚本解就可以了

```cpp
#include <iostream>
#include <cstdio>
#include <cstdint>

using namespace std;

void decipher(unsigned int num_rounds, uint64_t v[2]) {
    unsigned int i;
    uint64_t v0 = v[0], v1 = v[1];
    __int64 delta = 0x61C8864661C88647LL;
    __int64 sum = 0 - delta * num_rounds;
//    cout<<hex<<sum<<endl;
    for (i = 0; i < num_rounds; i++) {
        v1 -= (((v0 << 4) ^ (v0 >> 5)) + v0) ^ sum;
        sum += delta;
        v0 -= (((v1 << 4) ^ (v1 >> 5)) + v1) ^ sum;
    }
    v[0] = v0, v[1] = v1;
//    cout<<sum<<endl;
}

int main() {
    uint64_t v[2] = {0xAD4BB459940692AALL, 0x5665FD4EC447C6C9LL};
    unsigned int r = 32;
    decipher(r, v);
    std::cout << std::hex << v[0] << std::endl;
    std::cout << std::hex << v[1] << std::endl;
    //3638643936356561
    //6534356237643435
}
//68d965ea e45b7d45

//ae569d8654d7b54e
```

将输出结果转换成对应字母或数字然后按小端序重新排列得到结果