一些正则表达式随记
===

通过一些例子来学习正则表达式摘录，js正则函数match、exec、test、search、replace、split

> ⚠️ 创建了一个[新仓库](https://github.com/jaywcjlove/regexp-example)，方便专门搜集讨论正则相关内容。顺便将下面内容整理到了[新仓库](https://github.com/jaywcjlove/regexp-example)。=> [`@jaywcjlove/regexp-example`](https://github.com/jaywcjlove/regexp-example)

<!--idoc:ignore:start-->
<!-- TOC -->

- [一些匹配方法](#一些匹配方法)
  - [去除首尾的](#去除首尾的)
  - [javascript:; 、javascript:void(0)](#javascript-javascriptvoid0)
  - [匹配](#匹配)
  - [匹配一些字符](#匹配一些字符)
  - [关键字符替换](#关键字符替换)
  - [替换参数中的值](#替换参数中的值)
  - [匹配括号内容](#匹配括号内容)
  - [调换](#调换)
  - [字符串截取](#字符串截取)
  - [浏览器版本](#浏览器版本)
  - [转义 HTML 标签](#转义-html-标签)
- [验证](#验证)
  - [小数点后几位验证](#小数点后几位验证)
  - [密码强度正则](#密码强度正则)
  - [校验中文](#校验中文)
  - [包含中文正则](#包含中文正则)
  - [由数字、26个英文字母或下划线组成的字符串](#由数字26个英文字母或下划线组成的字符串)
  - [身份证号正则](#身份证号正则)
  - [校验日期](#校验日期)
  - [校验文件后缀](#校验文件后缀)
  - [用户名正则](#用户名正则)
  - [整数正则](#整数正则)
  - [数字正则](#数字正则)
  - [Email正则](#email正则)
  - [传真号码](#传真号码)
  - [手机号码正则](#手机号码正则)
  - [URL正则](#url正则)
  - [域名正则表达式](#域名正则表达式)
  - [Mac地址匹配](#mac地址匹配)
  - [浮点数正则表达式](#浮点数正则表达式)
  - [IPv4地址正则](#ipv4地址正则)
  - [日期格式化yyyy-MM-dd正则](#日期格式化yyyy-mm-dd正则)
  - [十六进制颜色正则](#十六进制颜色正则)
  - [QQ号码正则](#qq号码正则)
  - [微信号正则](#微信号正则)
  - [车牌号正则](#车牌号正则)
  - [颜色值校验](#颜色值校验)
- [工具](#工具)

<!-- /TOC -->
<!--idoc:ignore:end-->

## 一些匹配方法

### 去除首尾的

```js
//去除首尾的‘/’
input = input.replace(/^\/*|\/*$/g,'');
```

### javascript:; 、javascript:void(0)

```js
'javascript:;'.match(/^(javascript\s*\:|#)/);
//["javascript:", "javascript:", index: 0, input: "javascript:;"]
```

### 匹配  

```js
var str = "access_token=dcb90862-29fb-4b03-93ff-5f0a8f546250; refresh_token=702f4815-a0ff-456c-82ce-24e4d7d619e6; account_uid=1361177947320160506170322436";
str.match(/account_uid=([^\=]+(\;)|(.*))/ig);
//=> ["account_uid=1361177947320160506170322436"]
```

### 匹配一些字符

```js
var str = 'asdf html-webpack-plugin for "index/index.html" asdfasdf';
str.match(/html-webpack-plugin for \"(.*)\"/ig);
console.log(RegExp.$1) //=>index/index.html
```

### 关键字符替换

```js
'css/[hash:8].index-index.css'.replace(/\[(?:(\w+):)?(contenthash|hash)(?::([a-z]+\d*))?(?::(\d+))?\]/ig,'(.*)');
//=> css/(.*).index-index.css
```

### 替换参数中的值

```js
var str  = '<!DOCTYPE html><html manifest="../../cache.manifest" lang="en"><head><meta charset="UTF-8">';
str.replace(/<html[^>]*manifest="([^"]*)"[^>]*>/,function(word){
   return word.replace(/manifest="([^"]*)"/,'manifest="'+url+'"');
}).replace(/<html(\s?[^\>]*\>)/,function(word){
    if(word.indexOf('manifest')) return word;
    return word.replace('<html','<html manifest="'+url+'"');
});
//原：<!DOCTYPE html><html manifest="../../cache.manifest" lang="en"><head><meta charset="UTF-8">
//替换成=> <!DOCTYPE html><html manifest="cache.manifest" lang="en"><head><meta charset="UTF-8">
```

### 匹配括号内容

```js
'max_length(12)'.match(/^(.+?)\((.+)\)$/)
// ["max_length(12)", "max_length", "12", index: 0, input: "max_length(12)"]
'hello(world)code(js)javascirpt'.match(/\((\w*)+?\)/gmi);
// => ["(world)", "(js)"]
```

### 调换

```js
var name = "Doe, John"; 
name.replace(/(\w+)\s*, \s*(\w+)/, "$2 $1"); 
//=> "John Doe"
```

### 字符串截取

```js 
var str = 'asfdf === sdfaf ##'
str.match(/[^===]+(?=[===])/g) // 截取 ===之前的内容

str.replace(/\n/g,'')  // 替换字符串中的 \n 换行字符
```

### 浏览器版本

```js
navigator.userAgent.match(/chrome\/([\d]+)\.([\d]+)\.([\d]+)\.([\d]+)/i);
//=> ["Chrome/64.0.3282.167", "64", "0", "3282", "167", index: 87, input: "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_3) Ap…L, like Gecko) Chrome/64.0.3282.167 Safari/537.36"]
```

### 转义 HTML 标签

```js
'<style> body {margin: 0;}</style> <div>my string</div>'.replace( /[<>](?:(lt|gt|nbsp|#\d+);)?/g, (a, b) => {
  if(b) return a;
  else {
    return { '<': '&lt;', '>':'&gt;',}[a]
  }
})
```

## 验证

### 小数点后几位验证

```js
// 精确到1位小数
/^[1-9][0-9]*$|^[1-9][0-9]*\.[0-9]$|^0\.[0-9]$/.test(1.2);

// 精确到2位小数
/^[0-9]+(.[0-9]{2})?$/.test(1.221);
```

### 密码强度正则


```js 
// 必须是包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间。
/^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$/.test("weeeeeeeW2");
//密码强度正则，最少6位，包括至少1个大写字母，1个小写字母，1个数字，1个特殊字符
/^.*(?=.{6,})(?=.*\d)(?=.*[A-Z])(?=.*[a-z])(?=.*[!@#$%^&*? ]).*$/.test("diaoD123#");
//输出 true
```

### 校验中文

```js 
/^[\u4e00-\u9fa5]{0,}$/.test("但是d"); //false
/^[\u4e00-\u9fa5]{0,}$/.test("但是"); //true
/^[\u4e00-\u9fa5]{0,}$/.test("但是"); //true
```

### 包含中文正则

```js
/[\u4E00-\u9FA5]/.test("但是d") //true
```

### 由数字、26个英文字母或下划线组成的字符串

```js 
/^\w+$/.test("ds2_@#"); // false
```

### 身份证号正则

```js
//身份证号（18位）正则
/^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$/.test("42112319870115371X");
//输出 false
```

### 校验日期

“yyyy-mm-dd“ 格式的日期校验，已考虑平闰年。

```js 
//日期正则，简单判定,未做月份及日期的判定
var dP1 = /^\d{4}(\-)\d{1,2}\1\d{1,2}$/;
//输出 true
console.log(dP1.test("2017-05-11"));
//输出 true
console.log(dP1.test("2017-15-11"));
//日期正则，复杂判定
var dP2 = /^(?:(?!0000)[0-9]{4}-(?:(?:0[1-9]|1[0-2])-(?:0[1-9]|1[0-9]|2[0-8])|(?:0[13-9]|1[0-2])-(?:29|30)|(?:0[13578]|1[02])-31)|(?:[0-9]{2}(?:0[48]|[2468][048]|[13579][26])|(?:0[48]|[2468][048]|[13579][26])00)-02-29)$/;
//输出 true
console.log(dP2.test("2017-02-11"));
//输出 false
console.log(dP2.test("2017-15-11"));
//输出 false
console.log(dP2.test("2017-02-29"));
// true
```


### 校验文件后缀

```js
  var strRegex = "(.jpg|.gif|.txt)";
  var re=new RegExp(strRegex);
  if (re.test(str)){
    
  }
/(.jpg|.gif)+(\?|\#|$)/.test('a/b/c.jpgsss'); //=> false
/(.jpg|.gif)+(\?|\#|$)/.test('a/b/c.jpg?'); //=> true
```

### 用户名正则

```js
//用户名正则，4到16位（字母，数字，下划线，减号）
/^[a-zA-Z0-9_-]{4,16}$/.test("diaodiao");
//输出 true
```

### 整数正则

```js
/^\d+$/.test("42");    //正整数正则  -> 输出 true
/^-\d+$/.test("-42");  //负整数正则  -> 输出 true
/^-?\d+$/.test("-42"); //整数正则  -> 输出 true

/^[0-9]+$/.test(25.5455) //正整数正则  -> 输出 false
// 浮点数
/^(?:[-+])?(?:[0-9]+)?(?:\.[0-9]*)?(?:[eE][\+\-]?(?:[0-9]+))?$/.test(0.2)
```

### 数字正则

可以是整数也可以是浮点数

```js
/^\d*\.?\d+$/.test("42.2");     //正数正则  -> 输出 true
/^-\d*\.?\d+$/.test("-42.2");   //负数正则 -> 输出 true
/^-?\d*\.?\d+$/.test("-42.2");  //数字正则 -> 输出 true
```

### Email正则

```js
//Email正则
/^([A-Za-z0-9_\-\.])+\@([A-Za-z0-9_\-\.])+\.([A-Za-z]{2,4})$/.test("wowohoo@qq.com");
//输出 true

// 1.邮箱以a-z、A-Z、0-9开头，最小长度为1.
// 2.如果左侧部分包含-、_、.则这些特殊符号的前面必须包一位数字或字母。
// 3.@符号是必填项
// 4.右则部分可分为两部分，第一部分为邮件提供商域名地址，第二部分为域名后缀，现已知的最短为2位。
//   最长的为6为。
// 5.邮件提供商域可以包含特殊字符-、_、.
/^[a-z0-9]+([._\\-]*[a-z0-9])*@([a-z0-9]+[-a-z0-9]*[a-z0-9]+.){1,63}[a-z0-9]+$/.test("wowohoo@qq.com");
```

### 传真号码

```js
// 国家代码(2到3位)-区号(2到3位)-电话号码(7到8位)-分机号(3位)
/^(([0\+]\d{2,3}-)?(0\d{2,3})-)(\d{7,8})(-(\d{3,}))?$/.test('021-5055455')
```

### 手机号码正则

```js
//手机号正则
/^1[34578]\d{9}$/.test("13611778887");
//输出 true

//* 13段：130、131、132、133、134、135、136、137、138、139
//* 14段：145、147
//* 15段：150、151、152、153、155、156、157、158、159
//* 17段：170、176、177、178
//* 18段：180、181、182、183、184、185、186、187、188、189
//* 国际码 如：中国(+86)
/^((\+?[0-9]{1,4})|(\(\+86\)))?(13[0-9]|14[57]|15[012356789]|17[03678]|18[0-9])\d{8}$/.test("13611778887");
```


### URL正则

```js
//URL正则
/^((https?|ftp|file):\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/.test("http://wangchujiang.com");
//输出 true

//获取url中域名、协议正则 'http://xxx.xx/xxx','https://xxx.xx/xxx','//xxx.xx/xxx'
/^(http(?:|s)\:)*\/\/([^\/]+)/.test("http://www.baidu.com");

/^((http|https):\/\/(\w+:{0,1}\w*@)?(\S+)|)(:[0-9]+)?(\/|\/([\w#!:.?+=&%@!\-\/]))?$/.test('https://www.baidu.com/s?wd=@#%$^&%$#')

// 必须有协议 
/^[a-zA-Z]+:\/\//.test("http://www.baidu.com");
```


### 域名正则表达式

```js
/^([a-zA-Z0-9]([a-zA-Z0-9\-]{0,61}[a-zA-Z0-9])?\.)+[a-zA-Z]{2,6}$/.test('blog.csdn.net');
// 输出 true
```

### Mac地址匹配

```js
/^([0-9a-fA-F][0-9a-fA-F]:){5}([0-9a-fA-F][0-9a-fA-F])$/.test('dc:a9:04:77:37:20');
// 输出 true
```

### 浮点数正则表达式

```js
/[-+]?(?:\b[0-9]+(?:\.[0-9]*)?|\.[0-9]+\b)(?:[eE][-+]?[0-9]+\b)?/.test(+334.4443434343e3);
//输出 true
```

### IPv4地址正则

```js
//ipv4地址正则
/^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/.test("192.168.130.199");
//输出 true
```

### 日期格式化yyyy-MM-dd正则

```js
/(19|20)\d\d([- /.])(0[1-9]|1[012])\2(0[1-9]|[12][0-9]|3[01])/.test('2019-09-12')
//输出 true
```

### 十六进制颜色正则

```js
//RGB Hex颜色正则
/^#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$/.test("#b8b8b8");
//输出 true
```

### QQ号码正则

```js
//QQ号正则，5至11位
/^[1-9][0-9]{4,10}$/.test("398188661");//输出 true
```

### 微信号正则

```js
//微信号正则，6至20位，以字母开头，字母，数字，减号，下划线
/^[a-zA-Z]([-_a-zA-Z0-9]{5,19})+$/.test("jslite"); //输出 true
```

### 车牌号正则

```js
//车牌号正则
/^[京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领A-Z]{1}[A-Z]{1}[A-Z0-9]{4}[A-Z0-9挂学警港澳]{1}$/.test("沪B99116") //输出 true
```

### 颜色值校验

```js
// HEX 颜色正则
/^#?([0-9a-fA-F]{3}|[0-9a-fA-F]{6})$/.test("#ccb2b2")
```

## 工具

- [Regular Expression Library](http://regexlib.com/)
- [Regular expression visualizer using railroad diagrams](https://regexper.com/)
- [Online regex tester. Full highlighting of regex syntax. Very useful. Javascript regex used.](http://myregexp.com/)
- [正则表达式在线测试工具](http://regex.zjmainstay.cn/)
- [Regulex：JavaScript正则表达式展示器](https://jex.im/regulex/)
- [寻找完美的 URL 验证正则表达式](https://mathiasbynens.be/demo/url-regex)
