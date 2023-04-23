---
title: Hexo中的LeanCloud后端使用教程
date: 2023-4-22
tags: 经验分享
typora-root-url: ..
---
## 操作流程

1. ~~创建LeanCloud国际版用户（实测国内用QQ邮箱申请后，foxmail邮箱也可以重新申请国际版新账号），并创建应用，最好用国际版，否则国内版本需要备案之类的比较麻烦 https://leancloud.app/~~（后来证实需要一直科学上网才能上传表单，因此还是注册国内版就好）

   （点击邮箱验证时需要科学上网）

   ![0](/images/Hexo中的LeanCloud后端使用教程/0.png)

2. 进入应用，点击“创建Class”

   ![1](/images/Hexo中的LeanCloud后端使用教程/1.png)

3. 进入后选择对应的访问权限

   ![2](/images/Hexo中的LeanCloud后端使用教程/2.png)

这里，设置权限的场景可以参考这篇专栏 [云开发的数据库权限机制解读丨云开发101 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/82842893) 

因为做数据调研，调研者只需要采集信息，不需要用户看到或修改其他用户的数据，所以可以限制权限为“仅创建者或管理者可读写数据”即可，如图所示

![3](/images/Hexo中的LeanCloud后端使用教程/3.png)

4. 初始化的类数据如图所示：

   ![4](/images/Hexo中的LeanCloud后端使用教程/4.png)

5. 设置相关的数据项，此处先省略

6. ~~在hexo博客项目根目录下git bash, 然后用如下指令安装leancloud依赖~~ (可以跳过，使用cdn加载即可)

   ```shell
   npm install leancloud-storage --save
   ```

   ![5](/images/Hexo中的LeanCloud后端使用教程/5.png)

7. ~~然后就可以发现在package.json里出现了leancloud的相关配置，说明安装完成 (~~可以跳过，使用cdn加载即可)

   ![6](/images/Hexo中的LeanCloud后端使用教程/6.png)

8. 在markdown或html文件中编写相关的js和html代码，需要注意以下几点：

   (1) **在博客开头处设置加载leancloud的cdn资源的脚本**

   ```js
   <script src="https://cdn.jsdelivr.net/npm/leancloud-storage/dist/av-min.js"></script>
   ```

   (2) **经过上述设置后无需在后面初始化AV时重新定义变量AV**(因为加载cdn的时候会自动定义AV，如果像下面这样手动定义，就会把原来的cdn资源中定义好的变量覆盖了)

   下面是错误示范：

   ```js
   ...
   var AV = require('leancloud-storage');//错误之处
   var APP_ID = '<your app id>';
   var APP_KEY = '<your app key>';
   AV.init({
     appId: APP_ID,
     appKey: APP_KEY
   });
   ... 
   ```

   正确示范：

   ```js
   ...
   var APP_ID = '<your app id>';
   var APP_KEY = '<your app key>';
   AV.init({
     appId: APP_ID,
     appKey: APP_KEY,
     serverURLs: '这里复制对应app的rest api 服务地址'
   });
   ... 
   ```


   (3) 点击“设置”-“应用凭证”，要进去复制app id和app key，否则也无法直接使用

   (4) 另外，serverURLs也要设置好，国内版一定要设，否则cdn加载的js会出错；国际版的可以不设置，但是不用科学上网会无法上传表单。因此可以直接使用国内版，加上AV.init()中设置serverURLs即可

![11](/images/Hexo中的LeanCloud后端使用教程/11.png)

9. 最后，放出所有的JS代码：

   ```JS
   <script>
     // 初始化leancloud的应用ID和应用Key
     var APP_ID = 'your app id';
     var APP_KEY = 'your app key';
     AV.init({
       appId: APP_ID,
       appKey: APP_KEY，
       serverURLs: '这里复制对应app的rest api 服务地址'
     });

     // 获取html表单元素
     var form = document.getElementById('myForm');

     // 监听表单的提交事件
     form.addEventListener('submit', function(event) {
       // 阻止表单的默认提交行为
       event.preventDefault();

       // 获取表单中的输入值
       var result = form.elements['result'].value;

       // 创建一个leancloud的对象类，用于存储表单信息
       var FormInfo = AV.Object.extend('form_data');

       // 创建一个FormInfo的实例
       var formInfo = new FormInfo();

       // 设置实例的属性值
       formInfo.set('result', result);

       // 将实例保存到leancloud的后端服务中
       formInfo.save().then(function (formInfo) {
         // 成功保存后，打印日志并显示提示信息
         console.log('保存成功，对象ID为' + formInfo.id);
         alert('提交成功，感谢您的反馈！');
       }, function (error) {
         // 失败保存后，打印错误信息并显示提示信息
         console.error(error);
         alert('提交失败，请重试或者联系管理员！');
       });
     });
   </script>
   ```

   ​    其中

   ```js
       var FormInfo = AV.Object.extend('FormInfo');
   ```

   ​    表示从类FormInfo继承了一个子类，这个类就是我们在LeanCloud应用中创建的类名

   ​     

   ```js
       formInfo.set('result', result);
   ```

   ​    前者为在数据库设置好的key，后者为从表单获取的value，key也就是我们在类中设置的属性值，也就是某一列。下面是通过前端获取表单信息，并返回到leancloud数据库的结果：

   ![8](/images/Hexo中的LeanCloud后端使用教程/8.png)



​	在写demo的时候，没有必要通过hexo发布，可以直接通过vs code的live server插件开启本地端口直接查看js的代码是否正确，亲测使用该插件是可以调用leancloud接口的。

![10](/images/Hexo中的LeanCloud后端使用教程/10.png)				 

​	另外，很重要的一点是，要善用浏览器的debug功能，打开开发者工具，然后在可能有问题的地方设置断点，比自己瞎改代码快很多。因为js代码的特性，出错也一般不会报错，反馈就会很弱，所以debug非常重要。

![9](/images/Hexo中的LeanCloud后端使用教程/9.png)



## 避坑总结

1. 要用浏览器开发者工具debug，不然导入cdn的js出错，网页是不会有任何报错的，改代码就相当于盲人摸象一样效率低下了
2. 要多浏览官方的开发者文档，并注意内容的时效性
3. 如果访问博客的群体以国内为主，那还是推荐使用LeanCloud的国内版，因为访问无需科学上网。但是需要注意设置好serverURLs
4. cdn加载leancloud的js和npm本地导入js都是可行的，但注意不要两者一起用，否则自己导入后重新定义变量AV会覆盖cdn加载的。我认为cdn加载的已经够用了，没必要使用本地导入的，需要安装且容易出问题

## 后记

**为什么要采用这套方案？**

​	那么，基本demo跑通之后，才是开始介绍我选择Hexo + Leancloud这套方案的原因。因为现在国内做问卷基本都是被问卷星之类的霸占，而问卷星上提供的基本服务无法上传音视频等较大的文件，而付费服务又过于昂贵（企业版动辄2000元一个月的费用，令学生党望而生畏），并且问卷排布也有一定限制。这就促生我使用本地博客，加上后端服务进行问卷调查的想法。由于github.io的域名和服务器以及leancloud都是可以免费使用的，那么问卷的费用就可以大大节省，且问卷系统发布之后，可以接近无期限的使用，复用性也很高，下次换个问卷的话，仅需重新写一篇博客，然后对文件路径以及html的排版和js稍作调整，即可重新发布新的问卷。

**如何用这套方案进行问卷？**

​	这里另开一帖，专门介绍我是如何使用Hexo+Leancloud进行问卷调查的...