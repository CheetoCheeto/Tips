# 关于前端开发中遇到的坑，记录长记性

* iPhone X适配

    1.采用了viewport-fit的meta标签作为适配方案 ```viewport-fit:cover``` 来填充屏幕,
    使用constant(safe-area-inset-bottom)  env(safe-area-inset-bottom)来避开危险区域

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

    1.不同内核下，表现不同，图片导出时有可能是透明图，在option中宽高乘2可解，但也会造成部分浏览器图片大小为四分之一

    2.无法将background导出为canvas。改轮子可以解决，见[这里](https://github.com/niklasvh/html2canvas/issues/265)

    3.没有开启跨域的图片也是无法变为canvas的，服务器开跨域即可，即res中含有access-control-allow-orgin：*

    4.emoji无法转为canvas，暂时没想到办法，用了个◾️replace掉了emoji

    5.总的来说，想要完美使用html2canvas，改轮子是必不可少的一步，针对哪些问题的具体操作，google上有很详细的方式

* safari时间戳转换日期会慢一天（chorme实行夏时令原因，1986年到1991的时间戳转换实行夏时令）

* ajax 回调中使用window.open（）会被浏览器判定为广告从而被拦截

* 大于等于 12px 的奇数字号在Android上垂直居中表现怪异（偏上），可以用一下方法解决

    1.字体调整为大于12px的偶数字体同时```border:1px solid transparent```

    2.利用transform:scale & transition 解决

## 一些小提示

* ```:checked```伪类可以被用于```checkbox、radio、option(select)```元素
* CSS伪类选择器中 ```not()```括号中的选择器也占权值
* flex 布局 + margin-right/margin-left：auto 在某些情况有奇效
* img标签无论如何**都**会被加载
* 没有用到的CSS元素 和 【父】元素```display:none```的元素的bg**不**会被加载
* 跨域请求中的简单请求与非简单请求[会发送option请求]
* 行内元素垂直方向```margin，padding```无效，但```padding```会增加元素高却不会影响其他元素
* 元素被设置为浮动元素后，```display```属性为```block```同时像```inline```元素一样有包裹性
* 文档流内块级元素```width```默认值为100%；由父元素的```content```决定；脱离文档流的元素```float，position```宽度变为```auto```
* 关于JavaScript中定时器的[工作原理](https://johnresig.com/blog/how-javascript-timers-work/#postcomment)

## 一些函数

* 一个图片预加载函数
```js
    /**
        * @param {Object} sources
        * @param {Function} callback
    */
    function preLoadImages(sources, callback) {
        var cacheImages = {};
        var index = 0;
        var attCount = Object.getOwnPropertyNames(sources).length;
        for (imgItem in sources) {
            cacheImages[imgItem] = new Image();
            cacheImages[imgItem].onload = function () {
                index++;
                if (index == attCount) {
                    images = cacheImages;
                    if (typeof callback === "function") {
                        callback();
                    }
                }
            }
            cacheImages[imgItem].src = sources[imgItem];
        }
    }
```

* 防抖与节流函数
```js
    function debounce(fn) {
        let timeout = null; // 创建一个标记用来存放定时器的返回值
        return function () {
            clearTimeout(timeout); // 每当用户输入的时候把前一个 setTimeout clear 掉
            timeout = setTimeout(() => { // 然后又创建一个新的 setTimeout, 这样就能保证输入字符后的 interval 间隔内如果还有字符输入的话，就不会执行 fn 函数
            fn.apply(this, arguments);
            }, 500);
        };
    }
    function sayHi() {
        console.log('防抖成功');
    }
    var inp = document.getElementById('inp');
    inp.addEventListener('input', debounce(sayHi)); // 防抖

    function throttle(fn) {
        let canRun = true; // 通过闭包保存一个标记
        return function () {
            if (!canRun) return; // 在函数开头判断标记是否为true，不为true则return
            canRun = false; // 立即设置为false
            setTimeout(() => { // 将外部传入的函数的执行放在setTimeout中
            fn.apply(this, arguments);
            // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。当定时器没有执行的时候标记永远是false，在开头被return掉
            canRun = true;
            }, 500);
        };
    }
    function sayHi(e) {
        console.log(e.target.innerWidth, e.target.innerHeight);
    }
    window.addEventListener('resize', throttle(sayHi));
```


* QQ打开H5 title不会变化问题
    手动创建iframe，添加到body后移除可以触发QQ刷新title操作，但是这种方法对于直接点击对话中的链接打开的H5无效，扫码与空间打开有效，暂不知道QQ的处理逻辑

* 禁止移动端H5滑动回弹
```js
    let startY = 0;
    let enabled = false;
    let supportsPassiveOption = false;
    try {
        var opts = Object.defineProperty({}, 'passive', {
            get: function () {
                supportsPassiveOption = true;
            }
        });
        window.addEventListener('test', null, opts);
    } catch (e) {}
    var handleTouchmove = function (evt) {
        var el = evt.target;
        var zoom = window.innerWidth / window.document.documentElement.clientWidth;
        if (evt.touches.length > 1 || zoom !== 1) {
            return;
        }
        while (el !== document.body && el !== document) {
            var style = window.getComputedStyle(el);
            if (!style) {
                break;
            }
            if (el.nodeName === 'INPUT' && el.getAttribute('type') === 'range') {
                return;
            }
            var scrolling = style.getPropertyValue('-webkit-overflow-scrolling');
            var overflowY = style.getPropertyValue('overflow-y');
            var height = parseInt(style.getPropertyValue('height'), 10);
            var isScrollable = scrolling === 'touch' && (overflowY === 'auto' || overflowY === 'scroll');
            var canScroll = el.scrollHeight > el.offsetHeight;
            if (isScrollable && canScroll) {
                var curY = evt.touches
                    ? evt.touches[0].screenY
                    : evt.screenY;
                var isAtTop = (startY <= curY && el.scrollTop === 0);
                var isAtBottom = (startY >= curY && el.scrollHeight - el.scrollTop === height);
                if (isAtTop || isAtBottom) {
                    evt.preventDefault();
                }
                return;
            }
            el = el.parentNode;
        }
        evt.preventDefault();
    };
    var handleTouchstart = function (evt) {
        startY = evt.touches
            ? evt.touches[0].screenY
            : evt.screenY;
    };
    var enable = function () {
        window.addEventListener('touchstart', handleTouchstart, supportsPassiveOption
            ? {
                passive: false
            }
            : false);
        window.addEventListener('touchmove', handleTouchmove, supportsPassiveOption
            ? {
                passive: false
            }
            : false);
        enabled = true;
    };
    var disable = function () {
        window.removeEventListener('touchstart', handleTouchstart, false);
        window.removeEventListener('touchmove', handleTouchmove, false);
        enabled = false;
    };
    var isEnabled = function () {
        return enabled;
    };
    var testDiv = document.createElement('div');
    document.documentElement.appendChild(testDiv);
    testDiv.style.WebkitOverflowScrolling = 'touch';
    var scrollSupport = 'getComputedStyle' in window && window.getComputedStyle(testDiv)['-webkit-overflow-scrolling'] === 'touch';
    document.documentElement.removeChild(testDiv);
    if (scrollSupport) {
        enable();
    }
    export default {
        enable,
        disable,
        isEnabled
    }
```