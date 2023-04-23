---
title: 创建我的第一个App
date: 2021-10-17
tags: Android 经验分享
typora-root-url: ..
---
##### （1）首先，New ->New Project->Empty Activity 一路默认创建工程文件夹。

![image-20211017230552541](/images/创建我的第一个安卓App/image-20211017230552541.png)

![image-20211017230646889](/images/创建我的第一个安卓App/image-20211017230646889.png)

##### （2）再在**configuration**里添加**Android App**

![image-20211017230426065](/images/创建我的第一个安卓App/image-20211017230426065.png)

如下所示

![image-20211017230332355](/images/创建我的第一个安卓App/image-20211017230332355.png)

##### （3）Run！你的第一个App完成了

![image-20211017232213112](/images/创建我的第一个安卓App/image-20211017232213112.png)

---

#### 问题

然鹅，即使这么简单还是会出现一些问题

​				**报错 `License for package Android SDK Build-Tools 29.0.2not accepted`**

2. ![image-20211017231511880](/images/创建我的第一个安卓App/image-20211017231511880.png)

   **原因：**可能是你当前已有SDK库中的SDK和Project所用的版本不匹配

   **对策：**需要在SDK Manager里下载当前Project的所需安卓SDK版本，下载后重新运行即可。

   ![image-20211017231620276](/images/创建我的第一个安卓App/image-20211017231620276.png)

