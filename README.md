## 使用方法

```
import { 方法名称 } from "@/utils";
```

## 工具类

### PC端判断

```js
if (isPc) alert('这是PC端');
```

```js
/**
 * PC端判断
 * @returns {boolean}
 */
export function isPc() {
    let userAgentInfo = navigator.userAgent;
    let Agents = new Array("Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod");
    let flag = true;
    for (let v = 0; v < Agents.length; v++) {
        if (userAgentInfo.indexOf(Agents[v]) > 0) {
            flag = false;
            break
        }
    }
    return flag;
}
```

### 获取当前周几

别名：今天星期几

```js
getDayText() //五
```

```js
/**
 * 获取当前日期
 * @param date
 * @returns {string}
 */
export const getDayText = (date = new Date()) => {
        if (typeof (date) === 'number') {
            date = new Date(date);
        } else if (typeof (date) === 'string') {
            date = new Date(date.replace(/-/g, '/').replace(/\./g, '/'));
        }
        return '日一二三四五六'.charAt(date.getDay());
    };
```

### 时间问候语

别名：good morning

```js
/**
 * 时间问候语
 * @returns {string}
 */
export const greetTime = () => {
        const time = new Date()
        const hour = time.getHours()
        return hour < 9 ? '早上好' : hour <= 11 ? '上午好' : hour <= 13 ? '中午好' : hour < 20 ? '下午好' : '晚上好'
    }
```

### Date时间转数字

别名：时间转数字格式、时间戳

```js
parseTime(new Date(), '{y}{m}{d}') // 20211221
```

```js
/**
 * 时间转数字
 * @param time new Date()
 * @param cFormat 显示的格式
 * @returns {string|null}
 */
export function parseTime(time, cFormat) {
    if (arguments.length === 0 || !time) {
        return null;
    }
    const format = cFormat || '{y}-{m}-{d} {h}:{i}:{s}';
    let date;
    if (typeof time === 'object') {
        date = time;
    } else {
        if (typeof time === 'string') {
            if (/^[0-9]+$/.test(time)) {
                time = parseInt(time);
            } else {
                // support safari
                // https://stackoverflow.com/questions/4310953/invalid-date-in-safari
                time = time.replace(new RegExp(/-/gm), '/');
            }
        }

        if (typeof time === 'number' && time.toString().length === 10) {
            time = time * 1000;
        }
        date = new Date(time);
    }
    const formatObj = {
        y: date.getFullYear(),
        m: date.getMonth() + 1,
        d: date.getDate(),
        h: date.getHours(),
        i: date.getMinutes(),
        s: date.getSeconds(),
        a: date.getDay(),
    };
    const time_str = format.replace(/{([ymdhisa])+}/g, (result, key) => {
        const value = formatObj[key];
        // Note: getDay() returns 0 on Sunday
        if (key === 'a') {
            return ['日', '一', '二', '三', '四', '五', '六'][value];
        }
        return value.toString().padStart(2, '0');
    });
    return time_str;
}
```

### Date时间转汉字

别名：时间转汉字、时间戳

```js
formatTime(new Date()) // 刚刚
```

```js
/**
 * 时间转汉字
 * @param time
 * @param option
 * @returns {string}
 */
export function formatTime(time, option) {
    if (('' + time).length === 10) {
        time = parseInt(time) * 1000
    } else {
        time = +time
    }
    const d = new Date(time)
    const now = Date.now()

    const diff = (now - d) / 1000

    if (diff < 30) {
        return '刚刚'
    } else if (diff < 3600) {
        // less 1 hour
        return Math.ceil(diff / 60) + '分钟前'
    } else if (diff < 3600 * 24) {
        return Math.ceil(diff / 3600) + '小时前'
    } else if (diff < 3600 * 24 * 2) {
        return '1天前'
    }
    if (option) {
        // 必须使用 [ parseTime ] 这个函数
        // return parseTime(time, option)
    } else {
        return (
            d.getMonth() +
            1 +
            '月' +
            d.getDate() +
            '日' +
            d.getHours() +
            '时' +
            d.getMinutes() +
            '分'
        )
    }
}
```

### 价钱格式化

别名：给数字加逗号

```js
/**
 * 给数字加逗号
 * @param num
 * @returns {string}
 * @constructor
 */
export function NumberToMoney(num) {
    num = num.toFixed(2);
    num = parseFloat(num);
    num = num.toLocaleString();
    if (num.indexOf('.') < 0) {
        num += '.00';
    }
    return num;
}
```

### downFile文件下载

别名：前端下载图片

```js
let url = "https://avuejs.com/images/logo-bg.jpg";
downFile(url, 'logo.jpg');
```

```js
/**
 * downFile文件下载
 * @param url 下载链接
 * @param saveName 保存名字
 */
export function downFile(url, saveName) {
    if (typeof url == 'object' && url instanceof Blob) {
        url = URL.createObjectURL(url); // 创建blob地址
    }
    var aLink = document.createElement('a');
    aLink.href = url;
    aLink.download = saveName || ''; // HTML5新增的属性，指定保存文件名，可以不要后缀，注意，file:///模式下不会生效
    var event;
    if (window.MouseEvent) {
        event = new MouseEvent('click');
    } else {
        event = document.createEvent('MouseEvents');
        event.initMouseEvent('click', true, false, window, 0, 0, 0, 0, 0, false,
            false, false, false, 0, null);
    }
    aLink.dispatchEvent(event);
}
```

### 生成随机数

```js
randomString(5) // ZsAl6
```

```js
/**
 * 生成随机数
 * @param len 生成的长度
 * @returns {string}
 */
export const randomString = (len) => {
        len = len || 32;
        let chars = 'ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678';
        let maxPos = chars.length;
        let pwd = '';
        for (let i = 0; i < len; i++) {
            pwd += chars.charAt(Math.floor(Math.random() * maxPos));
        }
        return pwd;
    }
```

### 复制

```js
copyToClip('复制文本')
```

```js
/**
 * 复制
 * @param content 内容
 * @param info 复制成功的文案
 */
export function copyToClip(content, info) {
    let aux = document.createElement('input');
    aux.setAttribute('value', content);
    document.body.appendChild(aux);
    aux.select();
    document.execCommand('copy');
    document.body.removeChild(aux);
    if (info == null) {
        alert('复制成功');
    } else {
        alert(info);
    }
}
```

#### loadScript

别名：加载脚本、加载js、加载css、加载资源

```js
loadScript('js', 'xxx.js').then(() => {
//执行后的方法
})
loadScript('css', 'xxx.css').then(() => {
//执行后的方法
})
```

```js
/**
 * loadScript
 * @param type 类型
 * @param url 资源地址
 * @returns {Promise<unknown>}
 */
export const loadScript = (type = 'js', url) => {
        let flag = false;
        return new Promise((resolve) => {
            const head = document.getElementsByTagName('head')[0];
            head.children.forEach(ele => {
                if ((ele.src || '').indexOf(url) !== -1) {
                    flag = true;
                    resolve();
                }
            });
            if (flag) return;
            let script;
            if (type === 'js') {
                script = document.createElement('script');
                script.type = 'text/javascript';
                script.src = url;
            } else if (type === 'css') {
                script = document.createElement('link');
                script.rel = 'stylesheet';
                script.type = 'text/css';
                script.href = url;
            }
            head.appendChild(script);
            script.onload = function () {
                resolve();
            };
        });
    };
```

### 历史记录

别名：搜索历史、最近浏览

```js
/**
 * 最近浏览
 * @param item 单个元素
 * item => {
 *   id: 40,
 *   name: '第五个元素'
 * }
 */
export function setHistoryList(item) {
    const newViewNum = 8; // 要显示几条
    let HistoryList = [];
    if (window.localStorage.getItem('HistoryList') && window.localStorage.getItem('HistoryList') !== '[""]') {
        let newHistoryList = JSON.parse(window.localStorage.getItem('HistoryList'));
        let newArr = HistoryList.concat(newHistoryList)
        if (newArr.length > 0) {
            newArr.map((vo, key) => {
                // 搜索记录单个id 和 当前点击的元素
                if (vo.id === item.id) {
                    newArr.splice(key, 1)
                }
            })
        }
        newArr.unshift(item)
        if (newArr.length > newViewNum) {
            newArr.pop();
        }
        window.localStorage.setItem('HistoryList', JSON.stringify(newArr))
    } else {
        HistoryList.push(item);
        window.localStorage.setItem('HistoryList', JSON.stringify(HistoryList))
    }
}
```

### 防抖

```js
methods: {
    toggle:debounce(() => {
        console.log('防抖')
    },1000)
}
```

```js
/**
 * 防抖
 * @param func 方法名
 * @param wait 等待时间
 * @param immediate
 * @returns {function(...[*]=): *}
 */
export function debounce(func, wait, immediate) {
  let timeout, args, context, timestamp, result

  const later = function() {
    // 据上一次触发时间间隔
    const last = +new Date() - timestamp

    // 上次被包装函数被调用时间间隔 last 小于设定时间间隔 wait
    if (last < wait && last > 0) {
      timeout = setTimeout(later, wait - last)
    } else {
      timeout = null
      // 如果设定为immediate===true，因为开始边界已经调用过了此处无需调用
      if (!immediate) {
        result = func.apply(context, args)
        if (!timeout) context = args = null
      }
    }
  }

  return function(...args) {
    context = this
    timestamp = +new Date()
    const callNow = immediate && !timeout
    // 如果延时不存在，重新设定延时
    if (!timeout) timeout = setTimeout(later, wait)
    if (callNow) {
      result = func.apply(context, args)
      context = args = null
    }

    return result
  }
}
```

### 获取地址栏参数

别名：获取问号后面的参数

```js
// http://localhost:8080/page?a=123
getQueryObject() // {a:123}
```

```js
/**
 * 获取地址栏的参数
 * @param url
 * @returns {{}}
 */
export function getQueryObject(url) {
  url = url == null ? window.location.href : url
  const search = url.substring(url.lastIndexOf('?') + 1)
  const obj = {}
  const reg = /([^?&=]+)=([^?&=]*)/g
  search.replace(reg, (rs, $1, $2) => {
    const name = decodeURIComponent($1)
    let val = decodeURIComponent($2)
    val = String(val)
    obj[name] = val
    return rs
  })
  return obj
}
```

### base64转换为文件

```js
/**
 * base64转换为文件
 * @param url
 * @param filename
 * @returns {File}
 */
export function dataURLtoFile(url, filename) {
  let arr = url.split(",")
  let mime = arr[0].match(/:(.*?);/)[1]
  let bstr = atob(arr[1])
  let n = bstr.length
  let u8arr = new Uint8Array(n)
  while (n--) {
    u8arr[n] = bstr.charCodeAt(n)
  }
  return new File([u8arr], filename, { type: mime })
}
```

### 图片转换为base64

```js
/**
 * 图片转换为base64
 * @param url
 * @param callback
 * @param outputFormat
 */
export function getImgToBase64(url, callback, outputFormat) {
  let canvas = document.createElement("canvas")
  let ctx = canvas.getContext("2d")
  let img = new Image()
  img.crossOrigin = "Anonymous"
  img.onload = function () {
    canvas.height = img.height
    canvas.width = img.width
    ctx.drawImage(img, 0, 0)
    let dataURL = canvas.toDataURL(outputFormat || "image/png")
    callback(dataURL)
    canvas = null
  }
  img.src = url
}
```

## 方法类

> 包含了数组、字符串、对象的操作方法

### 修改数组里对象的key

别名：修改数组里对象的键名

```js
data: [
    {
        label: '标签',
        value: '值'
    }
]

// 使用后
newData: [
    {
        name: '标签',
        code: '值'
    }
]
```

```js
/**
 * 修改数组里对象的Key和值
 * @param arr
 * @returns {*}
 */
export function convertKey (arr) {
    return arr.map(item => ({
        label: item.name,
        value: item.code
    }))
}
```


## DOM类

### 跳转到某个元素

别名：滚动到某个div

```js
goTo() {
    goToDom(document.querySelector('#contentTop'), 0);
}
```

```js
/**
 * 跳转到某个元素
 * @param toEl
 * @param n
 */
export function goToDom(toEl, n) {
    let bridge = toEl;
    let body = document.body;
    let height = 0;
    do {
        height += bridge.offsetTop;
        bridge = bridge.offsetParent;
    } while (bridge !== body)
    // 增加动画
    window.scrollTo({
        top: height - n,
        behavior: 'smooth'
    })
}
```

### 切换元素

别名：切换class

```js
toggle() {
    toggleClass(document.body, 'dark')
}
```

```js
/**
 * 切换Class
 * @param element
 * @param className
 */
export function toggleClass(element, className) {
  if (!element || !className) {
    return
  }
  let classString = element.className
  const nameIndex = classString.indexOf(className)
  if (nameIndex === -1) {
    classString += '' + className
  } else {
    classString =
      classString.substr(0, nameIndex) +
      classString.substr(nameIndex + className.length)
  }
  element.className = classString
}
```


### 判断是否存在Class

别名：class是否存在

```js
/**
 * 判断是否存在某个Class
 * @param ele 元素
 * @param cls class名
 * @returns {boolean}
 */
export function hasClass(ele, cls) {
  return !!ele.className.match(new RegExp('(\\s|^)' + cls + '(\\s|$)'))
}
```

### 添加Class

```js
/**
 * 添加class
 * @param ele 元素
 * @param cls class名
 */
export function addClass(ele, cls) {
  if (!hasClass(ele, cls)) ele.className += ' ' + cls
}
```

### 删除Class

```js
/**
 * 删除Class
 * @param ele 元素
 * @param cls class名
 */
export function removeClass(ele, cls) {
  if (hasClass(ele, cls)) {
    const reg = new RegExp('(\\s|^)' + cls + '(\\s|$)')
    ele.className = ele.className.replace(reg, ' ')
  }
}
```
