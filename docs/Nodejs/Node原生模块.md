---
layout: default
title: Nodejs 原生模块
nav_order: 0
parent: Nodejs
permalink: /docs/nodejspackage
---

Node 的模块分为内置模块，第三方模块,这里讨论的也就是内置模块。


## 导出和导入
**module.exports（导出）：**
```
module.exports = {
    name:"onion",
    act(){
        console.log("之前我没得选，现在我想做一个好人")
    },
    age:24
}
```

**require（导入）：**
```
const info = require("./a.js")
console.log(ldh.age)//25
console.log(ldh.name)//onion
ldh.act();
//或者使用解构赋值
const {name} = require("./a.js") //只接受name的值
console.log(name)//onion
```
**Tips：**
- `module.exports = { name：‘onion’}` 和 `module.exports.name = 'onion'`的区别;前者是创建的新的对象，将原对象覆盖。后者是在原对象上添加一个新的 `name` 属性。
- `require（）`方法的返回值其实就是引入模块的 `module.exports` 对象。`require` 方法加载其他模块时，会执行被加载模块中的代码。



## http 模块
> `http` 意为传输协议（客户端以什么样的形式发送数据给服务器端 ↔ 服务器端以什么样的形式发送数据给客户端）

**http.createServer()（搭建一个服务器）：**
```
//1.引入http模块，用于搭建服务器。
const http = require("http");
//2.创建服务，传入一个回调函数.当有请求进来的时候，该回调就会执行。
//req:请求对象  包含请求信息和对应的方法
//res:响应对象  包含响应信息和对应的方法
let app = http.createServer((req,res)=>{
 //3.当有请求进来，我们给浏览器一个响应。
  res.end("<h1>hello,boy</h1>");
});
//4.添加端口监听   每一个软件启动都需要一个端口
app.listen(3000);
//这是一条提示信息而已
console.log("服务已启动！");
```


## path 模块
> - `path` 模块包含了一系列处理和转换文件路径的工具集合，不同操作系统的路径分隔符是不同的。 
> - `window` 下是 `\`，`linux` 是 `/`。
> - 大多数情况下在 `node` 中我们选择的是绝对路径，因为相对路径相对的是我们命令行所打开的目录.

**path.jion()（拼接路径）：**
```
const fs = require("fs");
//引入path路径
const path = require("path")

console.log("手动拼接："+__dirname+"\\a.txt");
console.log("path模块："+path.join(__dirname,"a.txt"));

fs.readFile(path.join(__dirname,"a.txt"),"utf-8",function (err,data) {
    if (err) {
        console.log(err);
        return;
    }
    console.log(data);
})
```

**Tips:**
- `__dirname` 返回当前目录的父级目录 ， 不属于path模块，每个自定义模块都有
- `__filename` 返回当前文件的绝对路径，包含文件名。不属于path模块，每个自定义模块都有
- `path.extname(path)` 返回路径中的文件的后缀名。
- `path.basename(path，[ext])` 返回文件的文件名（包含后缀）,如果指定了第二个参数，则表示 将该后缀删除。







## fs 模块
**fs.readFile（文件读取）：**
```
fs.readFile(filename,[encoding],[callback(error,data)])
//第一个必选参数:表示读取的文件名。
//第二个参数:是可选的，表示文件字符编码（例如：Utf-8）。
//第三个参数`callback`是回调函数，用于接收文件的内容（回调函数的两个参数 error 和 data ， err 表示错误的提示，有则出错无则没有错误；data 是文件内容。 如果指定 encoding ， data是一个解析后的字符串，否则表示二进制数据）。


//引入fs模块
const fs = require('fs');
//引入path模块
const path = require('path');
//文件路径
const filePath = path.join(__dirname,'File.txt')
const filePath1 = path.join(__dirname,'File1.txt')
fs.readFile(filePath,'utf8',function(err,data){
    console.log(data);
});
```


**fs.writeFile（写入文件）：**
```
fs.writeFile(filename,data,[options],callback)
//第一个必选参数:表示读取的文件名
//第二个参数:要写的数据
//第三个参数:是一个对象，如下↓
encoding {String | null} default='utf-8'
mode {Number} default=438(aka 0666 in Octal)
flag {String} default='w'


// 写入文件内容（如果文件不存在会创建一个文件）
// 写入时会先清空文件
fs.writeFile(filePath, '写入成功', function(err) {
    if (err) {
    //如果出错，抛出错误
        throw err;
    }
    // 写入成功后读取测试
    var data=fs.readFileSync(filePath, 'utf-8');
    console.log(data);
});
```
**appendFile（文件追加）：**

```
fs.appendFile(filename, data, [options], callback)
//第一个必选参数:表示读取的文件名
//第二个参数:可以是任意字符串或者缓存
//第三个参数 option 是一个对象，与write的区别就是[options]的flag默认值是”a”，所以它以追加方式写入数据.

// 写入文件内容（如果文件不存在会创建一个文件）
fs.appendFile(filePath, '追加的内容', function(err) {
    if (err) {
        throw err;
    }
    // 写入成功后读取测试
    var data=fs.readFileSync(filePath, 'utf-8');
    console.log(data);
});

```

文章出处：https://juejin.cn/post/7043604543883444260#heading-13