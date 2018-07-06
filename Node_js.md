#Node.js笔记01
##1.1 Node.js是什么
- Node.js是基于Chrome JavaScript运行时建立的一个平台，实际上它是对Google Chrome V8引擎进行了封装，它主要用于创建快速的、可扩展的网络应用。Node.js采用事件驱动和非阻塞I/O模型，使其变得轻量和高效，非常适合构建运行在分布式设备的数据密集型的实时应用。

- 对运行于浏览器的JavaScript来说，浏览器就是JavaScript代码的解析器，而Node.js则是服务器端的JavaScript代码解析器，存在于服务器端的JavaScript代码由Node.js来解析和运行。

- JavaScript解析器只是JavaScript代码运行的一种环境，浏览器是JavaScript运行的一种环境，浏览器为JavaScript提供了操作DOM对象和window对象等的接口。Node.js也是JavaScript运行的一种环境，Node.js为JavaScript提供了操作文件、创建HTTP服务、 创建TCP/UDP服务等的接口，所以Node.js可以完成其他后端开发语言（如Python、PHP等）能完成的工作。
##1.2 Node.js环境
###1.2.1 windows操作系统下Node.js的安装
- 首先移步到官网，[官方地址](https://nodejs.org/en/),选择推荐版本，然后一路NEXT。

![](https://i.imgur.com/TjJevRM.jpg)

安装完成后，在windows控制台测试一下是否安装成功，
>node -v //输出当前node.js的版本号
显示版本号的话证明安装已经成功了。

- windows下常用到的操作命令
>mkdir filename //创建一个新文件
>dir //显示当前路径下的文件及文件夹
>node name.js //执行当前路径下的js文件
>notepad name.js //对当前路径下的某个js文件使用记事本进行编辑
>notepsd name.js //如果觉得使用记事本编辑太low，可以将习惯使用的文本编辑器的路径添加到环境变量中，比如我的sublime执行文件的路径
>//e:/sublime_text,那么在环境变量添加以上路径，直接执行.exe文件的名称+.js文件名称就可以用sublime编辑器进行编辑了。
##1.3 Node.js模块和包
- 模块，模块就是Node.js封装的一些JS代码，分别实现相应的功能，比如EXPRESS,FS等等。
- 包的概念，包可以将多个具有依赖关系的模块组织封装到一起，以方便管理。根据CommonJS规范规定，一个JavaScript文件就是一个模块，而包是一个文件夹，包内必须包含一个JSON文件，命名为package.json。一般情况下，包内的bin文件夹存放二进制文件，包内的lib文件夹存放JavaScript文件，包内的doc文件夹存放文档，包内的test文件夹存放单元测试。
- 包管理工具，npm是Node.js的包管理工具，可以通过npm来下载第三方包，以及管理本地的第三方包。
###1.3.1 详细的描述一下模块
- 模块并不抽象，我们创建的每一个.js文件都是一个Node.js模块，通过包的管理可以将这些.js文件组织到一起，并建立起依赖关系。
- 模块的组成，每一个模块都包含一个 .js文件、.json文件以及一个二进制模块文件.node
- 模块之间如何关联到一起呢，要通过在每一个模块中使用exports以及module.exports将模块中的函数和变量显示导出。以下分别用实例进行说明
- exports方式导出
```javascript
//导出的模块
function hello() {
    console.log('Hello');
}

function world() {
    console.log('World');
}
//以下将该模块中的两个函数导出
exports.hello_out = hello;
exports.world_out = world;

//使用上面导出函数的模块导出的函数
var hello = require("./modulename.js");//假设两个模块在同一文件夹，相对路径
hello.hello_out();
hello.world_out();

```

- module.exports方式导出
```javascript
//导出的模块
function Hello() {
    this.hello = function() {
        console.log('Hello');
    };

    this.world = function() {
        console.log('World');
    };
}

module.exports = Hello;

//引用导出模块的部分
var Hello = require('./mymodule');

var hello = new Hello();

hello.hello(); // >> Hello
hello.world(); // >> World
```

- 关于exports和module.exports之间的区别，[见这里](https://cnodejs.org/topic/5231a630101e574521e45ef8)，简单说就是module.exports是一个空对象，exports是对象的引用，引用可以通过赋值指向新的一块内存。require返回的是module.exports。