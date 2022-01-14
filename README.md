## 导航

- [工具类](#工具类)
    - [PC端判断](#PC端判断)
    - [价钱格式化](#价钱格式化)
    - [downFile文件下载](#downFile文件下载)
    - [生成随机数](#生成随机数)
    - [复制](#复制)
    - [loadScript加载脚本](#loadScript)
    - [历史记录](#历史记录)
    - [防抖](#防抖)
    - [获取地址栏参数](#获取地址栏参数)
    - [base64转换为文件](#base64转换为文件)
    - [图片转换为base64](#图片转换为base64)
    - [uuid生成唯一值](#uuid生成唯一值)
    - [大小写转换或首字母](#大小写转换或首字母)
    - [将阿拉伯数字翻译成中文的大写数字](#将阿拉伯数字翻译成中文的大写数字)
    - [将数字转换为大写金额](#将数字转换为大写金额)

- [时间类](#时间类)
    - [获取当前周几](#获取当前周几)
    - [时间问候语](#时间问候语)
    - [Date时间转数字](#Date时间转数字)
    - [Date时间转汉字](#Date时间转汉字)
    - [获取某月有多少天](#获取某月有多少天)
    - [获取某年的第一天](#获取某年的第一天)
    - [获取某年最后一天](#获取某年最后一天)
    - [获取某个日期是当年中的第几天](#获取某个日期是当年中的第几天)
    - [获取某个日期在这一年的第几周](#获取某个日期在这一年的第几周)
    - [把秒转化为天小时分钟秒](#把秒转化为天小时分钟秒)

- [方法类](#方法类)
    - [根据pid生成树形结构](#根据pid生成树形结构)
    - [寻找所有子节点](#寻找所有子节点)
    - [类型判断(字符串、数组、对象等)](#类型判断)
    - [修改数组里对象的key](#修改数组里对象的key)
    - [加减乘除（解决精度丢失问题）](#加减乘除)

- [DOM类](#DOM类)
    - [获取元素](#获取元素)
    - [跳转到某个元素](#跳转到某个元素)
    - [切换元素](#切换元素)
    - [获取兄弟节点](#获取兄弟节点)
    - [判断是否存在Class](#判断是否存在Class)
    - [添加Class](#添加Class)
    - [删除Class](#删除Class)
    - [创建a标签打开新页面（解决会被浏览器阻止的问题）](#创建a标签打开新页面)

## 使用方法

```
import { 方法名称 } from "@/utils";
```

## 工具类

### PC端判断

```js
if (isPc()) alert('这是PC端');
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
    let aLink = document.createElement('a');
    aLink.href = url;
    aLink.download = saveName || ''; // HTML5新增的属性，指定保存文件名，可以不要后缀，注意，file:///模式下不会生效
    let event;
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
export function loadScript(type = 'js', url) {
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
    }, 1000)
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

    const later = function () {
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

    return function (...args) {
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
    return new File([u8arr], filename, {type: mime})
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

### uuid生成唯一值

```js
console.log(uuid()) // dec79cc2-ecf7-ec28-4b34-678b60ab37da
```

```js
/**
 * 生成唯一值
 * @returns {string}
 */
export function uuid() {
    const s4 = () => {
        return Math.floor((1 + Math.random()) * 0x10000).toString(16).substring(1);
    };
    return s4() + s4() + '-' + s4() + '-' + s4() + '-' + s4() + '-' + s4() + s4() + s4();
}
```

### 大小写转换或首字母

```js
changeBigSmallCase('Hello', 5); // hello 
```

```js
/**
 * @param str
 * @param type 1:首字母大写  2:首页母小写  3:大小写转换  4:全部大写  5:全部小写
 * @returns {string|*}
 */
export function changeBigSmallCase(str, type) {
    type = type || 4
    switch (type) {
        case 1:
            return str.replace(/\b\w+\b/g, function (word) {
                return word.substring(0, 1).toUpperCase() + word.substring(1).toLowerCase();

            });
        case 2:
            return str.replace(/\b\w+\b/g, function (word) {
                return word.substring(0, 1).toLowerCase() + word.substring(1).toUpperCase();
            });
        case 3:
            return str.split('').map(function (word) {
                if (/[a-z]/.test(word)) {
                    return word.toUpperCase();
                } else {
                    return word.toLowerCase()
                }
            }).join('')
        case 4:
            return str.toUpperCase();
        case 5:
            return str.toLowerCase();
        default:
            return str;
    }
}
```

### 将阿拉伯数字翻译成中文的大写数字

```js
numberToChinese('10') // 十
```

```js
/**
 * 将阿拉伯数字翻译成中文的大写数字
 * @param num
 * @returns {string}
 */
export function numberToChinese(num) {
    let AA = new Array("零", "一", "二", "三", "四", "五", "六", "七", "八", "九", "十");
    let BB = new Array("", "十", "百", "仟", "萬", "億", "点", "");
    let a = ("" + num).replace(/(^0*)/g, "").split("."),
        k = 0,
        re = "";
    for (let i = a[0].length - 1; i >= 0; i--) {
        switch (k) {
            case 0:
                re = BB[7] + re;
                break;
            case 4:
                if (!new RegExp("0{4}//d{" + (a[0].length - i - 1) + "}$")
                    .test(a[0]))
                    re = BB[4] + re;
                break;
            case 8:
                re = BB[5] + re;
                BB[7] = BB[5];
                k = 0;
                break;
        }
        if (k % 4 == 2 && a[0].charAt(i + 2) != 0 && a[0].charAt(i + 1) == 0)
            re = AA[0] + re;
        if (a[0].charAt(i) != 0)
            re = AA[a[0].charAt(i)] + BB[k % 4] + re;
        k++;
    }

    if (a.length > 1) // 加上小数部分(如果有小数部分)
    {
        re += BB[6];
        for (let i = 0; i < a[1].length; i++)
            re += AA[a[1].charAt(i)];
    }
    if (re == '一十')
        re = "十";
    if (re.match(/^一/) && re.length == 3)
        re = re.replace("一", "");
    return re;
}
```

### 将数字转换为大写金额

```js
console.log(changeToChinese('1000')) // 壹仟元整
```

```js
/**
 * 将数字转换为大写金额
 * @param Num
 * @returns {string}
 */
export function changeToChinese(Num) {
    //判断如果传递进来的不是字符的话转换为字符
    if (typeof Num == "number") {
        Num = new String(Num);
    }
    ;
    Num = Num.replace(/,/g, "") //替换tomoney()中的“,”
    Num = Num.replace(/ /g, "") //替换tomoney()中的空格
    Num = Num.replace(/￥/g, "") //替换掉可能出现的￥字符
    if (isNaN(Num)) { //验证输入的字符是否为数字
        //alert("请检查小写金额是否正确");
        return "";
    }
    ;
    //字符处理完毕后开始转换，采用前后两部分分别转换
    let part = String(Num).split(".");
    let newchar = "";
    //小数点前进行转化
    for (let i = part[0].length - 1; i >= 0; i--) {
        if (part[0].length > 10) {
            return "";
            //若数量超过拾亿单位，提示
        }
        let tmpnewchar = ""
        let perchar = part[0].charAt(i);
        switch (perchar) {
            case "0":
                tmpnewchar = "零" + tmpnewchar;
                break;
            case "1":
                tmpnewchar = "壹" + tmpnewchar;
                break;
            case "2":
                tmpnewchar = "贰" + tmpnewchar;
                break;
            case "3":
                tmpnewchar = "叁" + tmpnewchar;
                break;
            case "4":
                tmpnewchar = "肆" + tmpnewchar;
                break;
            case "5":
                tmpnewchar = "伍" + tmpnewchar;
                break;
            case "6":
                tmpnewchar = "陆" + tmpnewchar;
                break;
            case "7":
                tmpnewchar = "柒" + tmpnewchar;
                break;
            case "8":
                tmpnewchar = "捌" + tmpnewchar;
                break;
            case "9":
                tmpnewchar = "玖" + tmpnewchar;
                break;
        }
        switch (part[0].length - i - 1) {
            case 0:
                tmpnewchar = tmpnewchar + "元";
                break;
            case 1:
                if (perchar != 0) tmpnewchar = tmpnewchar + "拾";
                break;
            case 2:
                if (perchar != 0) tmpnewchar = tmpnewchar + "佰";
                break;
            case 3:
                if (perchar != 0) tmpnewchar = tmpnewchar + "仟";
                break;
            case 4:
                tmpnewchar = tmpnewchar + "万";
                break;
            case 5:
                if (perchar != 0) tmpnewchar = tmpnewchar + "拾";
                break;
            case 6:
                if (perchar != 0) tmpnewchar = tmpnewchar + "佰";
                break;
            case 7:
                if (perchar != 0) tmpnewchar = tmpnewchar + "仟";
                break;
            case 8:
                tmpnewchar = tmpnewchar + "亿";
                break;
            case 9:
                tmpnewchar = tmpnewchar + "拾";
                break;
        }
        let newchar = tmpnewchar + newchar;
    }
    //小数点之后进行转化
    if (Num.indexOf(".") != -1) {
        if (part[1].length > 2) {
            // alert("小数点之后只能保留两位,系统将自动截断");
            part[1] = part[1].substr(0, 2)
        }
        for (i = 0; i < part[1].length; i++) {
            tmpnewchar = ""
            perchar = part[1].charAt(i)
            switch (perchar) {
                case "0":
                    tmpnewchar = "零" + tmpnewchar;
                    break;
                case "1":
                    tmpnewchar = "壹" + tmpnewchar;
                    break;
                case "2":
                    tmpnewchar = "贰" + tmpnewchar;
                    break;
                case "3":
                    tmpnewchar = "叁" + tmpnewchar;
                    break;
                case "4":
                    tmpnewchar = "肆" + tmpnewchar;
                    break;
                case "5":
                    tmpnewchar = "伍" + tmpnewchar;
                    break;
                case "6":
                    tmpnewchar = "陆" + tmpnewchar;
                    break;
                case "7":
                    tmpnewchar = "柒" + tmpnewchar;
                    break;
                case "8":
                    tmpnewchar = "捌" + tmpnewchar;
                    break;
                case "9":
                    tmpnewchar = "玖" + tmpnewchar;
                    break;
            }
            if (i == 0) tmpnewchar = tmpnewchar + "角";
            if (i == 1) tmpnewchar = tmpnewchar + "分";
            newchar = newchar + tmpnewchar;
        }
    }
    //替换所有无用汉字
    while (newchar.search("零零") != -1)
        newchar = newchar.replace("零零", "零");
    newchar = newchar.replace("零亿", "亿");
    newchar = newchar.replace("亿万", "亿");
    newchar = newchar.replace("零万", "万");
    newchar = newchar.replace("零元", "元");
    newchar = newchar.replace("零角", "");
    newchar = newchar.replace("零分", "");
    if (newchar.charAt(newchar.length - 1) == "元") {
        newchar = newchar + "整"
    }
    return newchar;
}
```

## 时间类

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
        const time = new Date();
        const hour = time.getHours();
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

### 获取某月有多少天

```js
console.log(getMonthOfDay('2021-2')) // 28
console.log(getMonthOfDay()) // 当前为2021-12月，即得到的是30
```

```js
/**
 * 获取某月有多少天
 * @param time
 * @returns {number}
 */
export function getMonthOfDay(time) {
    let date = new Date(time);
    let year = date.getFullYear();
    let mouth = date.getMonth() + 1;
    let days;

    let timeArr = [1, 3, 5, 7, 8, 9, 10, 12]
    //当月份为二月时，根据闰年还是非闰年判断天数
    if (mouth === 2) {
        days = year % 4 === 0 ? 29 : 28
    } else if (timeArr.includes(mouth)) {
        //月份为：1,3,5,7,8,10,12 时，为大月.则天数为31；
        days = 31
    } else {
        //其他月份，天数为：30.
        days = 30
    }
    return days
}
```

### 获取某年的第一天

```js
console.log(getFirstDayOfYear('2021')) // 2021-01-01 00:00:00
```

```js
/**
 * 获取某年的第一天
 * @param time
 * @returns {string}
 */
export function getFirstDayOfYear(time) {
    let year = new Date(time).getFullYear();
    return year + "-01-01 00:00:00";
}
```

### 获取某年最后一天

```js
console.log(getLastDayOfYear('2021')) // 2021-12-31 23:59:59
```

```js
/**
 * 获取某年最后一天
 * @param time
 * @returns {string}
 */
export function getLastDayOfYear(time) {
    let year = new Date(time).getFullYear();
    let dateString = year + "-12-01 00:00:00";
    // 要配合 [ getMonthOfDay ] 这个方法使用
    let endDay = getMonthOfDay(dateString);
    return year + "-12-" + endDay + " 23:59:59";
}
```

### 获取某个日期是当年中的第几天

```js
console.log(getDayOfYear('2021-12-13')) // 347
```

```js
/**
 * 获取某个日期是当年中的第几天
 * @param time
 * @returns {number}
 */
export function getDayOfYear(time) {
    // 要配合 [ getFirstDayOfYear ] 这个方法使用
    let firstDayYear = getFirstDayOfYear(time);
    let numSecond = (new Date(time).getTime() - new Date(firstDayYear).getTime()) / 1000;
    return Math.ceil(numSecond / (24 * 3600));
}
```

### 获取某个日期在这一年的第几周

```js
console.log(getDayOfYearWeek('2021-12-31')) // 53
```

```js
/**
 * 获取某个日期在这一年的第几周
 * @param time
 * @returns {number}
 */
export function getDayOfYearWeek(time) {
    // 要配合 [ getDayOfYear ] 这个方法使用
    let numDays = getDayOfYear(time);
    return Math.ceil(numDays / 7);
}
```

### 把秒转化为天小时分钟秒

```js
sToHs(60) // 1分钟0秒
```

```js
/**
 * 把秒转化为天小时分钟秒
 * @param seconds
 * @returns {string}
 */
export function sToHs(seconds){
  let daySec = 24 *  60 * 60;
  let hourSec=  60 * 60;
  let minuteSec = 60;
  let dd = Math.floor(seconds / daySec);
  let hh = Math.floor((seconds % daySec) / hourSec);
  let mm = Math.floor((seconds % hourSec) / minuteSec);
  let ss = seconds%minuteSec;
  if(dd > 0) {
    return dd + "天" + hh + "小时" + mm + "分钟"+ss+"秒";
  } else if (hh > 0) {
    return hh + "小时" + mm + "分钟"+ss+"秒";
  } else if (mm > 0) {
    return mm + "分钟"+ss+"秒";
  } else {
    return ss+"秒";
  }
}
```
## 方法类

> 包含了数组、字符串、对象的操作方法

### 根据pid生成树形结构

```js
/**
 * 根据pid生成树形结构
 *  @param { object } items 后台获取的数据
 *  @param { * } id 数据中的id
 *  @param { * } link 生成树形结构的依据
 */
export const createTree = (items, id = null, link = 'pid') =>{
    items.filter(item => item[link] === id).map(item => ({ ...item, children: createTree(items, item.id) }));
};
```

### 寻找所有子节点

```js
/**
 * 寻找所有子节点
 * @param id
 * @param data
 * @param pidName 父级键名
 * @param idName 子级自己的id
 * @param childrenName 包含子级的数组键名
 * @returns {[]}
 */
export function traceChildNode(id, data, pidName = 'parentId', idName = 'id', childrenName = 'children') {
    let arr = [];
    foreachTree(data, childrenName, (node) => {
        if (node[pidName] == id) {
            arr.push(node);
            arr = arr.concat(traceChildNode(node[idName], data, pidName, idName, childrenName));
        }
    });
    return arr;
}
```

### 类型判断

```js
/**
 * 是否是对象
 * @param input
 * @returns {boolean}
 */
export function isObject(input) {
    return Object.prototype.toString.call(input) === '[object Object]';
}
```

```js
/**
 * 是否是数组
 * @param input
 * @returns {boolean}
 */
export function isArray(input) {
    return input instanceof Array || Object.prototype.toString.call(input) === '[object Array]';
}
```

```js
/**
 * 是否是时间类型
 * @param input
 * @returns {boolean}
 */
export function isDate(input) {
    return input instanceof Date || Object.prototype.toString.call(input) === '[object Date]';
}
```

```js
/**
 * 是否是数字
 * @param input
 * @returns {boolean}
 */
export function isNumber(input) {
    return input instanceof Number || Object.prototype.toString.call(input) === '[object Number]';
}
```

```js
/**
 * 是否是字符串
 * @param input
 * @returns {boolean}
 */
export function isString(input) {
    return input instanceof String || Object.prototype.toString.call(input) === '[object String]';
}
```

```js
/**
 * 是否是布尔值
 * @param input
 * @returns {boolean}
 */
export function isBoolean(input) {
    return typeof input == 'boolean';
}
```

```js
/**
 * 是否是方法
 * @param input
 * @returns {boolean}
 */
export function isFunction(input) {
    return typeof input == 'function';
}
```

```js
/**
 * 是否为空
 * @param input
 * @returns {boolean}
 */
export function isNull(input) {
    return input === undefined || input === null;
}
```

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
export function convertKey(arr) {
    return arr.map(item => ({
        label: item.name,
        value: item.code
    }))
}
```

### 加减乘除

> 解决精度丢失问题

```js
/**
 * 加法函数（精度丢失问题）
 * @param { number } arg1
 * @param { number } arg2
 */
export function add(arg1, arg2) {
    let r1, r2, m;
    try { r1 = arg1.toString().split(".")[1].length } catch (e) { r1 = 0 }
    try { r2 = arg2.toString().split(".")[1].length } catch (e) { r2 = 0 }
    m = Math.pow(10, Math.max(r1, r2));
    return (arg1 * m + arg2 * m) / m
}
```

```js
/**
 * 减法函数（精度丢失问题）
 * @param { number } arg1
 * @param { number } arg2
 */
export function sub(arg1, arg2) {
    let r1, r2, m, n;
    try { r1 = arg1.toString().split(".")[1].length } catch (e) { r1 = 0 }
    try { r2 = arg2.toString().split(".")[1].length } catch (e) { r2 = 0 }
    m = Math.pow(10, Math.max(r1, r2));
    n = (r1 >= r2) ? r1 : r2;
    return Number(((arg1 * m - arg2 * m) / m).toFixed(n));
}
```

```js
/**
 * 乘法函数（精度丢失问题）
 * @param { number } num1
 * @param { number } num2
 */
export function mcl(num1,num2){
    let m=0,s1=num1.toString(),s2=num2.toString();
    try{m+=s1.split(".")[1].length}catch(e){}
    try{m+=s2.split(".")[1].length}catch(e){}
    return Number(s1.replace(".",""))*Number(s2.replace(".",""))/Math.pow(10,m);
}
```

```js
/**
 * 除法函数（精度丢失问题）
 * @param { number } num1
 * @param { number } num2
 */
export function division(num1,num2){
    let t1,t2,r1,r2;
    try{
        t1 = num1.toString().split('.')[1].length;
    }catch(e){
        t1 = 0;
    }
    try{
        t2=num2.toString().split(".")[1].length;
    }catch(e){
        t2=0;
    }
    r1=Number(num1.toString().replace(".",""));
    r2=Number(num2.toString().replace(".",""));
    return (r1/r2)*Math.pow(10,t2-t1);
}
```

## DOM类

### 获取元素

```js
export function $(selector) {
    let type = selector.substring(0, 1);
    if (type === '#') {
        if (document.querySelecotor) return document.querySelector(selector)
        return document.getElementById(selector.substring(1))
    } else if (type === '.') {
        if (document.querySelecotorAll) return document.querySelectorAll(selector)
        return document.getElementsByClassName(selector.substring(1))
    } else {
        return document['querySelectorAll' ? 'querySelectorAll' : 'getElementsByTagName'](selector)
    }
}
```

### 跳转到某个元素

别名：滚动到某个div

```js
goTo()
{
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
toggle()
{
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

### 获取兄弟节点

```js
/**
 * 获取兄弟节点
 * @param ele
 * @returns {[]}
 */
export function siblings(ele) {
    let chid = ele.parentNode.children, eleMatch = [];
    for (let i = 0, len = chid.length; i < len; i++) {
        if (chid[i] != ele) {
            eleMatch.push(chid[i]);
        }
    }
    return eleMatch;
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

### 创建a标签打开新页面

别名：window.open被浏览器拦截

```js
/**
 * window.open(url)打开链接被浏览器拦截解决方案
 * @param url
 */
export function addBlankPath(url) {
    const a = document.createElement('a')
    a.href = url  // 窗口的链接
    a.target = '_blank'
    document.body.appendChild(a)
    a.click()
    document.body.removeChild(a)
}
```
