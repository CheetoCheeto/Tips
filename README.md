# 关于前端开发中遇到的坑，记录长记性

iPhone X适配

1.采用了viewport-fit的meta标签作为适配方案 ```viewport-fit:cover``` 来填充屏幕

2.```@media only screen and (width: 375px) and (height: 690px){
        body {
            background: blue;
            }
    }```

iOS webview中，使用$(document)绑定事件会失效

iOS 存在position：fixed bottom：0的元素，虚拟键盘弹出时，位置会错[position修改为absolute可解]

Safari中input标签lineheight，设置后靠上，尽量使用padding[包括解决光标高度问题]

用于产生滚动回弹效果的-webkit-overflow-scrolling: touch在iOS/UIWebView上有很多坑[具体是啥我居然忘记了，不过推荐使用better-scroll]

父级元素带有transform属性会使子元素Position:fixed失效

iOS input时间选择默认带秒

iOS时间转换之间要用‘/’而不是‘-’[这个印象深刻，具体见 https://blog.csdn.net/qq_36850813/article/details/80003654 ]

iOS12 WebView中 多次点击不同的video标签 会导致应用闪退

-webkit-tap-highlight-color: transparent 去掉点击时产生的阴影

iOS拍摄的照片 在上传时部分会旋转 使用exif.js解决

## 一些小提示

1. :checked伪类可以被用于checkbox、radio、option(select)元素
2. CSS伪类选择器中 not()括号中的选择器也占权值
3. img标签无论如何都会被加载
4. 没有用到的CSS元素 和 【父】元素display为none的元素的bg【不】会被加载
5. 跨域请求中的简单请求与非简单请求[会发送option请求]
6. 行内元素垂直方向margin，padding无效，但padding会增加元素高却不会影响其他元素
7. 元素被设置为浮动元素后，display属性为block 同时像inline元素一样有包裹性
8. 文档流内块级元素Width默认值为100%；由父元素的content决定；脱离文档流的元素【float，position】宽度变为auto
