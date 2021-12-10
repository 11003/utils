## 使用方法

```
import { 方法名称 } from "@/utils";
```

## 工具类

### 时间格式化

别名：时间

```js
parseTime(new Date(), '{y}{m}{d}') //20211221
```

```js
/**
 * 时间格式化
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


### downFile文件下载

别名：前端下载图片

```js
let url = "https://avuejs.com/images/logo-bg.jpg";
downFile(url,'logo.jpg');
```

```js
/**
 * downFile文件下载
 * @param url 下载链接
 * @param saveName 保存名字
 */
export function downFile (url, saveName) {
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
