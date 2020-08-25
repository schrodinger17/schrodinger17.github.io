这国赛出的没什么意思，re就给了两道然后被打穿了，等了一天最后放出来一道智能合约？？？

# re

## z3

直接解方程组得到flag

```python
target = [0x00004F17, 0x00009CF6, 0x00008DDB, 0x00008EA6, 0x00006929, 0x00009911, 0x000040A2, 0x00002F3E, 0x000062B6,
          0x00004B82, 0x0000486C, 0x00004002, 0x000052D7, 0x00002DEF, 0x000028DC, 0x0000640D, 0x0000528F, 0x0000613B,
          0x00004781, 0x00006B17, 0x00003237, 0x00002A93, 0x0000615F, 0x000050BE, 0x0000598E, 0x00004656, 0x00005B31,
          0x0000313A, 0x00003010, 0x000067FE, 0x00004D5F, 0x000058DB, 0x00003799, 0x000060A0, 0x00002750, 0x00003759,
          0x00008953, 0x00007122, 0x000081F9, 0x00005524, 0x00008971, 0x00003A1D]
from z3 import *


def RecurOr(flags, models, pos=0):
    if (pos < len(flags) - 1):
        return Or(models[flags[pos]] != flags[pos], RecurOr(flags, models, pos + 1))
    else:
        return Or(models[flags[pos]] != flags[pos])


v = [Int('v%d' % (i + 46)) for i in range(42)]
flag = []
solver = Solver()

for c in v:
    solver.add(c >= 0x0)
    solver.add(c <= 0xff)
v46 = v[0]
v47 = v[1]
v48 = v[2]
v49 = v[3]
v50 = v[4]
v51 = v[5]
v52 = v[6]
v53 = v[7]
v54 = v[8]
v55 = v[9]
v56 = v[10]
v57 = v[11]
v58 = v[12]
v59 = v[13]
v60 = v[14]
v61 = v[15]
v62 = v[16]
v63 = v[17]
v64 = v[18]
v65 = v[19]
v66 = v[20]
v67 = v[21]
v68 = v[22]
v69 = v[23]
v70 = v[24]
v71 = v[25]
v72 = v[26]
v73 = v[27]
v74 = v[28]
v75 = v[29]
v76 = v[30]
v77 = v[31]
v78 = v[32]
v79 = v[33]
v80 = v[34]
v81 = v[35]
v82 = v[36]
v83 = v[37]
v84 = v[38]
v85 = v[39]
v86 = v[40]
v87 = v[41]

solver.add(target[0] == 34 * v49 + 12 * v46 + 53 * v47 + 6 * v48 + 58 * v50 + 36 * v51 + v52)
solver.add(target[1] == 27 * v50 + 73 * v49 + 12 * v48 + 83 * v46 + 85 * v47 + 96 * v51 + 52 * v52)
solver.add(target[2] == 24 * v48 + 78 * v46 + 53 * v47 + 36 * v49 + 86 * v50 + 25 * v51 + 46 * v52)
solver.add(target[3] == 78 * v47 + 39 * v46 + 52 * v48 + 9 * v49 + 62 * v50 + 37 * v51 + 84 * v52)
solver.add(target[4] == 48 * v50 + 14 * v48 + 23 * v46 + 6 * v47 + 74 * v49 + 12 * v51 + 83 * v52)
solver.add(target[5] == 15 * v51 + 48 * v50 + 92 * v48 + 85 * v47 + 27 * v46 + 42 * v49 + 72 * v52)
solver.add(target[6] == 26 * v51 + 67 * v49 + 6 * v47 + 4 * v46 + 3 * v48 + 68 * v52)
solver.add(target[7] == 34 * v56 + 12 * v53 + 53 * v54 + 6 * v55 + 58 * v57 + 36 * v58 + v59)
solver.add(target[8] == 27 * v57 + 73 * v56 + 12 * v55 + 83 * v53 + 85 * v54 + 96 * v58 + 52 * v59)
solver.add(target[9] == 24 * v55 + 78 * v53 + 53 * v54 + 36 * v56 + 86 * v57 + 25 * v58 + 46 * v59)
solver.add(target[10] == 78 * v54 + 39 * v53 + 52 * v55 + 9 * v56 + 62 * v57 + 37 * v58 + 84 * v59)
solver.add(target[11] == 48 * v57 + 14 * v55 + 23 * v53 + 6 * v54 + 74 * v56 + 12 * v58 + 83 * v59)
solver.add(target[12] == 15 * v58 + 48 * v57 + 92 * v55 + 85 * v54 + 27 * v53 + 42 * v56 + 72 * v59)
solver.add(target[13] == 26 * v58 + 67 * v56 + 6 * v54 + 4 * v53 + 3 * v55 + 68 * v59)
solver.add(target[14] == 34 * v63 + 12 * v60 + 53 * v61 + 6 * v62 + 58 * v64 + 36 * v65 + v66)
solver.add(target[15] == 27 * v64 + 73 * v63 + 12 * v62 + 83 * v60 + 85 * v61 + 96 * v65 + 52 * v66)
solver.add(target[16] == 24 * v62 + 78 * v60 + 53 * v61 + 36 * v63 + 86 * v64 + 25 * v65 + 46 * v66)
solver.add(target[17] == 78 * v61 + 39 * v60 + 52 * v62 + 9 * v63 + 62 * v64 + 37 * v65 + 84 * v66)
solver.add(target[18] == 48 * v64 + 14 * v62 + 23 * v60 + 6 * v61 + 74 * v63 + 12 * v65 + 83 * v66)
solver.add(target[19] == 15 * v65 + 48 * v64 + 92 * v62 + 85 * v61 + 27 * v60 + 42 * v63 + 72 * v66)
solver.add(target[20] == 26 * v65 + 67 * v63 + 6 * v61 + 4 * v60 + 3 * v62 + 68 * v66)
solver.add(target[21] == 34 * v70 + 12 * v67 + 53 * v68 + 6 * v69 + 58 * v71 + 36 * v72 + v73)
solver.add(target[22] == 27 * v71 + 73 * v70 + 12 * v69 + 83 * v67 + 85 * v68 + 96 * v72 + 52 * v73)
solver.add(target[23] == 24 * v69 + 78 * v67 + 53 * v68 + 36 * v70 + 86 * v71 + 25 * v72 + 46 * v73)
solver.add(target[24] == 78 * v68 + 39 * v67 + 52 * v69 + 9 * v70 + 62 * v71 + 37 * v72 + 84 * v73)
solver.add(target[25] == 48 * v71 + 14 * v69 + 23 * v67 + 6 * v68 + 74 * v70 + 12 * v72 + 83 * v73)
solver.add(target[26] == 15 * v72 + 48 * v71 + 92 * v69 + 85 * v68 + 27 * v67 + 42 * v70 + 72 * v73)
solver.add(target[27] == 26 * v72 + 67 * v70 + 6 * v68 + 4 * v67 + 3 * v69 + 68 * v73)
solver.add(target[28] == 34 * v77 + 12 * v74 + 53 * v75 + 6 * v76 + 58 * v78 + 36 * v79 + v80)
solver.add(target[29] == 27 * v78 + 73 * v77 + 12 * v76 + 83 * v74 + 85 * v75 + 96 * v79 + 52 * v80)
solver.add(target[30] == 24 * v76 + 78 * v74 + 53 * v75 + 36 * v77 + 86 * v78 + 25 * v79 + 46 * v80)
solver.add(target[31] == 78 * v75 + 39 * v74 + 52 * v76 + 9 * v77 + 62 * v78 + 37 * v79 + 84 * v80)
solver.add(target[32] == 48 * v78 + 14 * v76 + 23 * v74 + 6 * v75 + 74 * v77 + 12 * v79 + 83 * v80)
solver.add(target[33] == 15 * v79 + 48 * v78 + 92 * v76 + 85 * v75 + 27 * v74 + 42 * v77 + 72 * v80)
solver.add(target[34] == 26 * v79 + 67 * v77 + 6 * v75 + 4 * v74 + 3 * v76 + 68 * v80)
solver.add(target[35] == 34 * v84 + 12 * v81 + 53 * v82 + 6 * v83 + 58 * v85 + 36 * v86 + v87)
solver.add(target[36] == 27 * v85 + 73 * v84 + 12 * v83 + 83 * v81 + 85 * v82 + 96 * v86 + 52 * v87)
solver.add(target[37] == 24 * v83 + 78 * v81 + 53 * v82 + 36 * v84 + 86 * v85 + 25 * v86 + 46 * v87)
solver.add(target[38] == 78 * v82 + 39 * v81 + 52 * v83 + 9 * v84 + 62 * v85 + 37 * v86 + 84 * v87)
solver.add(target[39] == 48 * v85 + 14 * v83 + 23 * v81 + 6 * v82 + 74 * v84 + 12 * v86 + 83 * v87)
solver.add(target[40] == 15 * v86 + 48 * v85 + 92 * v83 + 85 * v82 + 27 * v81 + 42 * v84 + 72 * v87)
solver.add(target[41] == 26 * v86 + 67 * v84 + 6 * v82 + 4 * v81 + 3 * v83 + 68 * v87)

while (solver.check() == sat):
    flag = ""
    model = solver.model()
    for i in range(42):
        flag += chr(model[v[i]].as_long())
    print(flag)
    solver.add(RecurOr(v, model))
# flag{7e171d43-63b9-4e18-990e-6e14c2afe648}
```

## hyperthreading

输入长度为42位的flag，然后创建了新的线程，里面对输入进行了处理，还有反调试，最后进行比较

![image-20200821091520232](_images\2020-ciscn\image-20200821091520232.png)

处理过程有一些花指令，去了花指令之后可以看到处理的过程

![image-20200821092126482](_images\2020-ciscn\image-20200821092126482.png)

取出其中一位，右移两位和左移六位之后的结果进行异或，然后再异或0x23

![image-20200821092313770](_images\2020-ciscn\image-20200821092313770.png)

出来的结果加上0x23

![image-20200821092356200](_images\2020-ciscn\image-20200821092356200.png)

索引加一，移向下一位，直接上脚本

```python
target = [0xDD, 0x5B, 0x9E, 0x1D, 0x20, 0x9E, 0x90, 0x91, 0x90, 0x90, 0x91, 0x92, 0xDE, 0x8B, 0x11, 0xD1, 0x1E, 0x9E,
          0x8B, 0x51, 0x11, 0x50, 0x51, 0x8B, 0x9E, 0x5D, 0x5D, 0x11, 0x8B, 0x90, 0x12, 0x91, 0x50, 0x12, 0xD2, 0x91,
          0x92, 0x1E, 0x9E, 0x90, 0xD2, 0x9F]
for i in range(len(target)):
    target[i] = ((target[i] - 0x23) & 0xff) ^ 0x23
    target[i] = ((target[i] << 2) | (target[i] >> 6)) & 0xff
# print(target)
flag = ""
for i in target:
    flag += chr(i)
print(flag)

# flag{a959951b-76ca-4784-add7-93583251ca92}
```



# misc

## 电脑被黑

diskgenius挂载虚拟磁盘disk_dump，恢复被删除数据

![image-20200821093045254](_images\2020-ciscn\image-20200821093045254.png)

恢复出了被删除的文件，但是是乱码，打开demo发现文件头elf，这个程序对flag文件进行了处理

![image-20200821093316861](_images\2020-ciscn\image-20200821093316861.png)

上脚本逆向处理一下

```python
v4 = 34
v5 = 0
target = [0x44, 0x2A, 0x03, 0xE5, 0x29, 0xA3, 0xAF, 0x62, 0x05, 0x31, 0x4E, 0xF3, 0xD6, 0xEB, 0x90, 0x66,
          0x24, 0x5C, 0xB7, 0x92, 0xF6, 0xD7, 0x4D, 0x0B, 0x6A, 0x41, 0xA3, 0x85, 0xEF, 0x90, 0x5A, 0x7E,
          0x5B, 0xEC, 0xC1, 0xF0, 0xD4, 0x61, 0x12, 0x12, 0x45, 0xEB, 0xB8]
flag = ""
for i in target:
    v6 = i
    flag += chr(((v6 ^ v4) - v5) & 0xff)
    v4 += 34
    v5 = (v5 + 2) & 0xF
print(flag)
# flag{e5d7c4ed-b8f6-4417-8317-b809fc26c047}
```



## WamaCry1

简易的勒索病毒，打包处理过，解包之后，查看一下student_unpacked，除了一些根本没用的按钮和输出之外，还调用了son.exe

![image-20200821093629676](_images\2020-ciscn\image-20200821093629676.png)

在解包之后的文件中找到这个程序，分析一下程序逻辑

![image-20200821093904054](_images\2020-ciscn\image-20200821093904054.png)

获取电脑名字，生成公钥和私钥对flag进行加密

![image-20200821093750996](_images\2020-ciscn\image-20200821093750996.png)

连接服务器，ip和端口如图，向服务器发送电脑名和私钥

得到ip以后扫描端口，发现开放8080是tomcat的默认管理页面，尝试弱口令tomcat/tomcat进去项目管理页面，构造war包上传拿到shell，在/tmp/key/文件夹下发现私钥及勒索病毒服务器端

在服务器段拿到生成的私钥文件和服务器端运行的程序，查看私钥发现经过了处理，分析服务器端程序，先开放端口建立连接

![image-20200821094420639](_images\2020-ciscn\image-20200821094420639.png)

接收先发送的电脑名称，创建以电脑名命名的文件

![image-20200821094550871](_images\2020-ciscn\image-20200821094550871.png)

rsa私钥有很多行，逐行接收，最关键的处理就是buf和1进行异或，这个异或只影响整数的最后一位，对应转换成的字符串的第一个字符，所以对私钥文件的第一列异或1还原回来



