# 关于前端开发中遇到的坑，记录长记性




### iPhone X适配

1.采用了viewport-fit的meta标签作为适配方案 ```viewport-fit:cover``` 来填充屏幕

2.
    ```@media only screen and (width: 375px) and (height: 690px){
        body {
            background: blue;
            }
    }```

### iOS webview中，使用$(document)绑定事件会失效

### iOS 存在position：fixed bottom：0的元素，虚拟键盘弹出时，位置会错[position修改为absolute可解]

### Safari中input标签lineheight，设置后靠上，尽量使用padding[包括解决光标高度问题]

### 用于产生滚动回弹效果的-webkit-overflow-scrolling: touch在iOS/UIWebView上有很多坑[具体是啥我居然忘记了，不过推荐使用better-scroll]

### 父级元素带有transform属性会使子元素Position:fixed失效

### iOS input时间选择默认带秒

### iOS时间转换之间要用‘/’而不是‘-’[这个印象深刻，具体见 https://blog.csdn.net/qq_36850813/article/details/80003654 ]

### iOS12 WebView中 多次点击不同的video标签 会导致应用闪退

### -webkit-tap-highlight-color: transparent 去掉点击时产生的阴影

### iOS拍摄的照片 在上传时部分会旋转 使用exif.js解决