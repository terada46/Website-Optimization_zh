# 网站性能优化项目

此项目来自优达学诚(UDACITY)的前端开发纳米学位课程的网站优化项目。此项目是基于Javascript编写的，项目的目标是利用课程中学习的技术来优化关键渲染路径，并使目标页面和功能尽可能快的渲染。


## 运行（访问）网站应用
#### 在电脑上检查
1. 无须进行任何安装，只需要一个网页浏览器即可简单运行。建议使用CHROME浏览器。
2. 运行方法：点击**Clone or download**下载ZIP压缩包至本地解压后双击index.html，或直接将`index.html`拖拽到浏览器打开；或可直接访问[在线版本](https://terada46.github.io/Website-Optimization_zh/ "Website Optimization zh")。

#### 在手机上检查
1. 依上述将代码库下载导出。
2. 运行一个本地服务器，以便在你的手机上检查这个站点。

    ```
     $> cd /你的工程目录
     $> python -m SimpleHTTPServer 8080
    ```

#### 远程访问
1. 打开浏览器，访问 localhost:8080
2. 下载 ngrok 并将其安装在你的工程根目录下，让你的本地服务器能够被远程访问。

    ```
      $> cd /你的工程目录
      $> ./ngrok http 8080
    ```
	

## 优化目标

1. 优化`index.html`在移动设备和桌面上的**PageSpeed**分数，使其至少达到90分。
2. 对`views/js/main.js`进行优化，使`views/pizza.html`在滚动时保持 60fps 的帧速。
3. 对`views/js/main.js`进行优化，实现当点击`views/pizza.html`页面上的 pizza 尺寸滑块，调整 pizza 大小的时间小于5毫秒。


## 优化方法

#### 目标1: 提升index.html的PageSpeed分数
* 给导致网站长时间打不开的字体错误链接加上`https:`，使其正常访问，将此link标签移动至html文件末尾。
* print.css文件阻塞渲染，它并不需要在网页加载时获取，将其声明为`media="print"` 。
* 优化尺寸过大的pizzeria.jpg至合适大小，以更快完成所需图片资源下载。
* 向引用外部分析代码`analytics.js`文件的script标记添加异步关键字`async` 。同时按错误警报修复`analytics.js`文件链接为https协议。
* 分析`style.css`中代码简短，直接提取放在内联样式中，减少外部文件下载次数。

#### 目标2: 保持pizza.html滚动保持60fps
* `pizza.html`滚动时发生了强制布局同步，是由滚动时运行的`updatePositions`函数中for循环(main.js:495)执行读取`document.body.scrollTop`后又立刻设置元素样式引发的。`document.body.scrollTop`不需要在每次循环中执行，将其提前到for循环前，定义一个变量获取即可。

    ```
        var temScrollTop = document.body.scrollTop / 1250;
    ```
* 将背景生成的pizza数量减少至48个，无需生成超出屏幕范围外的pizza。 (main.js:522)
	```
	    for (var i = 0; i < 48; i++)
	```
* 对`scroll`和`DOMContentLoaded`的监听事件处理函数里使用`requestAnimationFrame优化动画帧。


#### 目标3: 调整pizza大小时间小于5毫秒
* 原`changePizzaSizes`函数的for循环中执行`determineDX`函数造成强制布局同步，从`sizeSwitcher`函数中提取switch条件语句至`changePizzaSizes`函数内，通过size值直接按比例改变元素尺寸，以取代处理工作繁琐的determineDX函数。


## 版权
代码及文档内容采用自 [优达学城网站优化实战项目](https://github.com/udacity/cn-frontend-development-advanced/raw/master/Website%20Optimization_zh.zip)。创建者为Udacity的Cameron Pittman。


## 相关阅读
如果你对此项目感兴趣，希望了解更多相关信息，可以访问[优达学城论坛](https://discussions.youdaxue.com/)阅读。