---
title: Live Server：本地HTML代码预览插件
date: 2023-4-22
tags: 经验分享
typora-root-url: ..

---
**Live Server：本地HTML代码预览插件**

功能：实现在本地端口预览HTML,可以快速检查JS代码功能

使用步骤：

1.  vs code 扩展栏即可直接下载

    ![1](/images/live server/1.png)

2.  下载完毕需要重启vs code

3.  打开HTML所在文件夹，将整个文件夹作为项目打开（否则无法直接使用这个插件）

    ![2](/images/live server/2.png)

4.  打开后选中想要测试的HTML,右键点击即可

    ![3](/images/live server/3.png)

5.  最后结果展示
    ![4](/images/live server/4.png)

    （需要注意，如果将hexo的markdown直接复制成HTML，文件根路径需要重新调整，因为HTML不支持markdown的typora-root-yrl的根路径设置）因此会出现前面的文本解析错误，音频加载不出来等问题

    ![5](/images/live server/5.png)
