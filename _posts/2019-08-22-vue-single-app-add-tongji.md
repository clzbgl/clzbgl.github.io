---
layout: post
title:  "Vue单页应用中追加百度统计"
date:   2019-08-22 22:47:42 +0800
categories: tech
typora-root-url: ..
---

Vue单页应用中追加百度统计分两步实现：

第一步，向index.html的header中追加统计脚本

项目根目录下的index.html或public目录下的index.html

```html
<script>
    var _hmt = _hmt || [];
    (function() {
        var hm = document.createElement("script");
        hm.src = "https://hm.baidu.com/hm.js?xxxxxxxxxxxxxxxxxxxxxxxxxxx";
        var s = document.getElementsByTagName("script")[0]; 
        s.parentNode.insertBefore(hm, s);
    })();
</script>
```

第二步，监听路由变化，发出pv统计

找到src/router目录下的index.js，找到定义router的地方，追加如下代码

```js
// vue router
router.beforeEach((to, from, next) => {
  // 统计代码
  // _hmt.push(['_trackPageview', pageURL]) 必须是以"/"（斜杠）开头的相对路径
  if (to.path) window._hmt.push(['_trackPageview', '/#' + to.fullPath])
  next()
})
```

验证，在浏览器的控制台中(Window系统按F12)查看如果有如下请求发送表示安装成功

![](/assets/img/2019/08/2019-08-22_224734.png)



参考：

- [vue spa中使用百度统计](https://juejin.im/post/5a98eef1f265da23a334a8fc)