最近有太多时间，这个比赛又比较简单，就干脆直接放脚本了

### beginners

#### beginner 1

base64

#### beginner 2

```python
target = [0x72, 0x74, 0x63, 0x70, 0x7b, 0x62, 0x6f, 0x62, 0x5f, 0x79, 0x6f, 0x75, 0x5f, 0x73, 0x75, 0x63, 0x6b, 0x5f,
          0x61, 0x74, 0x5f, 0x62, 0x65, 0x69, 0x6e, 0x67, 0x5f, 0x65, 0x6e, 0x63, 0x6f, 0x75, 0x72, 0x61, 0x67, 0x69,
          0x6e, 0x67, 0x7d]
flag = ''
for i in target:
    flag += chr(i)
print(flag)
# rtcp{bob_you_suck_at_being_encouraging}
```

### reverse-engineering

#### Fragile

```java
package com.company;

import static java.lang.System.exit;

import java.util.Scanner;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import javax.crypto.Cipher;
import javax.crypto.SecretKey;
import javax.crypto.SecretKeyFactory;
import javax.crypto.spec.DESKeySpec;

public class Main {

    public static void main(String[] var0) throws Exception {
        String flag = "h1_th3r3_1ts_m3";
        String theflag = "ÐdØÓ§åÍaèÒÁ¡";
        String f="";
        for (int i = 0; i < flag.length(); i++) {
            f += (char) ((int) (theflag.charAt(i)) - (int) (flag.charAt(i)));
        }
        System.out.println(f);
    }
}
// rtcp{h3y_1ts_n0t_b4d}
```

#### EZ

```
rtcp{tH1s_i5_4_r3aL_fL4g_s0_Do_sUbm1T_1t!}
```

#### PZ

```
rtcp{iT5_s1mPlY_1n_tH3_C0d3}
```

#### LEMON

```
rtcp{y34H_tHiS_a1nT_sEcuR3}
```

#### SQUEEZY

```python
import base64

en = 'HxEMBxUAURg6I0QILT4UVRolMQFRHzokRBcmAygNXhkqWBw='
enc = base64.b64decode(en)
key = "meownyameownyameownyameownyameownya"
flag = ''.join(chr(ord(a) ^b) for a, b in zip(key, enc))
print(flag)
# rtcp{y0u_L3fT_y0uR_x0r_K3y_bEh1nD!}
```

#### thedanzman

```python
import base64
import codecs

en = "=ZkXipjPiLIXRpIYTpQHpjSQkxIIFbQCK1FR3DuJZxtPAtkR"
en = en[::-1]
key = "nyameowpurrpurrnyanyapurrpurrnyanya"
key = codecs.encode(key, "rot_13")
d = codecs.encode(en, 'rot_13')
e = base64.b64decode(d)
f = ''.join(chr(ord(a) ^ b) for a, b in zip(key, e))
print(f)
# rtcp{n0w_tH4T_w45_m0r3_cH4lL3NgiNG}
```

#### Breakable

```java
package com.company;

import static java.lang.System.exit;

import java.util.Scanner;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import javax.crypto.Cipher;
import javax.crypto.SecretKey;
import javax.crypto.SecretKeyFactory;
import javax.crypto.spec.DESKeySpec;

public class Main {

    public static void main(String[] var0) throws Exception {
        String flag = "k33p_1t_in_pl41n";
        String theflag = "ÒdÝ¾¤¤¾ÙàåÐcÝÆ¥ÌÈáÏÜ¦aã";
        String f1="";
        String f2="";
        for (int i = 0; i < flag.length()-2; i++) {
            f1 += (char) ((int) (theflag.charAt(i+14)) - (int) (flag.charAt(i+2)));
        }
        System.out.println(f1);
        for (int i = 0; i < flag.length()-2; i++) {
            f2 += (char) ((int) (theflag.charAt(i)) - (int) (flag.charAt(i)));
        }
        System.out.println(f2);
    }
}
// rtcp{0mg_1m_s0_pr0ud_}
```



#### Bendy

```python
flag = "r34l_g4m3rs_eXclus1v3"
theflag = "ÄÑÓ¿ÂÒêáøz§è§ñy÷¦"
f1 = theflag[:9]
f2 = theflag[9:]
ff = ""
f = ""

for i in range(9):
    ff += chr(ord(f2[i]) - 20)
for i in range(9):
    ff += f1
for i in range(10, 15):
    f += chr(ord(ff[i - 3]) - ord(flag[i]))
f += " "
for i in range(7):
    f += chr(ord(ff[i]) - ord(flag[i]))
for i in range(15, 21):
    f += chr(ord(ff[i - 3]) - ord(flag[i - 3]))
print(f)
#rtcp{hop3_y0ur3_h4v1ng_fun}
```

#### Tough

```java
package com.company;

import java.util.HashMap;


public class Main {
    public static int[] realflag = {9, 4, 23, 8, 17, 1, 18, 0, 13, 7, 2, 20, 16, 10, 22, 12, 19, 6, 15, 21, 3, 14, 5, 11};
    public static int[] therealflag = {20, 16, 12, 9, 6, 15, 21, 3, 18, 0, 13, 7, 1, 4, 23, 8, 17, 2, 10, 22, 19, 11, 14, 5};
    public static HashMap<Integer, Character> theflags = new HashMap<>();
    public static HashMap<Integer, Character> theflags0 = new HashMap<>();
    public static HashMap<Integer, Character> theflags1 = new HashMap<>();
    public static HashMap<Integer, Character> theflags2 = new HashMap<>();
    public static boolean m = true;
    public static boolean g = false;

    public static void createMap(HashMap owo, String input, boolean uwu) {
        if (uwu) {
            for (int i = 0; i < input.length(); i++) {
                owo.put(realflag[i], input.charAt(i));
            }
        } else {
            for (int i = 0; i < input.length(); i++) {
                owo.put(therealflag[i], input.charAt(i));
            }
        }
    }

    public static void main(String[] var0) throws Exception {
        String flag = "ow0_wh4t_4_h4ckr_y0u_4r3";
        createMap(theflags0, flag, g);
        createMap(theflags2, flag, m);
        int[] thefinalflag = {157, 157, 236, 168, 160, 162, 171, 162, 165, 199, 169, 169, 160, 194, 235, 207, 227, 210, 157, 203, 227, 104, 212, 202};
        String s="";
        char[] f = new char[24];
        for (int p = 0; p < thefinalflag.length; p++) {
            if (thefinalflag[p] >= 156 && thefinalflag[p] <= 166)
                thefinalflag[p] -= 10;
            thefinalflag[p] -= theflags0.get(p);
//            s+=(char)(thefinalflag[p]);
        }
//        System.out.println(s);
        thefinalflag[8]+=10;
        thefinalflag[5]+=10;
//        System.out.println((char)thefinalflag[8]);
        for(int i=0;i<thefinalflag.length-3;i++)
        {
            int j=0;
            for(;j<realflag.length;j++)
            {
                if(realflag[j]==i)
                    break;
            }
            f[j]=(char)thefinalflag[i];
        }
        System.out.println(f);
        for(int i=thefinalflag.length-3;i<thefinalflag.length;i++)
        {
            int j=0;
            for(;j<therealflag.length;j++)
            {
                if(therealflag[j]==i)
                    break;
            }
            f[j]=(char)thefinalflag[i];
        }
        System.out.println(f);
    }
}
//rtcp{h3r3s_4_c0stly_fl4g_4you}
```

