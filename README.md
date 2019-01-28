# 关于前端开发中遇到的坑，记录长记性

* iPhone X适配

    1.采用了viewport-fit的meta标签作为适配方案 ```viewport-fit:cover``` 来填充屏幕

    2.```@media only screen and (width: 375px) and (height: 690px){
            body {
                background: blue;
                }
        }```

* iOS webview中，使用```$(document)```绑定事件会失效

* iOS 存在```position：fixed;bottom：0```的元素，虚拟键盘弹出时，位置会错(position修改为absolute可解)

* Safari中```input```标签```lineheight```，设置后靠上，尽量使用```padding```(包括解决光标高度问题)

* 用于产生滚动回弹效果的```-webkit-overflow-scrolling: touch```在iOS/UIWebView上有很多坑(具体是啥我居然忘记了，不过推荐使用better-scroll)

* 父级元素带有```transform```属性会使子元素```position:fixed```失效

* iOS ```input```时间选择默认带秒

* iOS时间转换之间要用‘/’而不是‘-’（这个印象深刻，具体见[这里](https://blog.csdn.net/qq_36850813/article/details/80003654))

* iOS12 WebView中 多次点击不同的```video```标签 会导致应用闪退

* ```-webkit-tap-highlight-color: transparent``` 去掉点击时产生的阴影

* iOS拍摄的照片 在上传时部分会旋转 使用exif.js解决

* html2canvas的一些大坑
    1. 不同内核下，表现不同，图片导出时有可能是透明图，在option中宽高乘2可解，但也会造成部分浏览器图片大小为四分之一

    2. 无法将background导出为canvas。修改原文件可以解决，见[这里](https://github.com/niklasvh/html2canvas/issues/265)

    3. 没有开启跨域的图片也是无法变为canvas的，服务器开跨域即可，即res中含有access-control-allow-orgin：*

    4. emoji无法转为canvas，暂时没想到办法，用了个◾️replace掉了emoji

## 一些小提示

* ```:checked```伪类可以被用于```checkbox、radio、option(select)```元素
* CSS伪类选择器中 ```not()```括号中的选择器也占权值
* img标签无论如何**都**会被加载
* 没有用到的CSS元素 和 【父】元素```display:none```的元素的bg**不**会被加载
* 跨域请求中的简单请求与非简单请求[会发送option请求]
* 行内元素垂直方向```margin，padding```无效，但```padding```会增加元素高却不会影响其他元素
* 元素被设置为浮动元素后，```display```属性为```block```同时像```inline```元素一样有包裹性
* 文档流内块级元素```width```默认值为100%；由父元素的```content```决定；脱离文档流的元素```float，position```宽度变为```auto```
* 关于JavaScript中定时器的[工作原理](https://johnresig.com/blog/how-javascript-timers-work/#postcomment)
