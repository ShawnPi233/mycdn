---
title: Quartus ii 9.0 破解过程
date: 2021-10-21
tags: Quartus
typora-root-url: ..
---

###### 步骤

1.将破解文件中的bin64的sys_cpt.dll复制到quartus的文件夹中的bin64文件夹进行覆盖(安装32位版本分别对应的是bin文件夹)

2.打开Quartus-Tools-License Setup

![image-20211021142113610](images/Quartus9 9.0最快激活方法/image-20211021142113610.png)

复制第一个地址到破解文件夹中的License.DAT(切记是第一个，不是全部！！！)

![image-20211021142153074](images/Quartus9 9.0最快激活方法/image-20211021142153074.png)

上下两处的HOSTID都要替换

![image-20211021142608675](images/Quartus9 9.0最快激活方法/image-20211021142608675.png)

3.将License.DAT文件夹复制到指定文件夹，然后再在License Setup中选择License file为该文件。

###### 坑：

1.在复制.dll文件前不要打开Quartus，否则会无法复制成功，因此上述操作顺序不可改变，不然就要重新启动才能复制

2.复制地址时，只复制第一个物理地址。