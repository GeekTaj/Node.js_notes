#Node.js基础
##一、模块机制
- 模块机制是Node.js一个优秀的特质，在Node启动的很短时间内，社区就已经贡献了大量的扩展库（模块）。其中很多是连接数据库或是其他软件的驱动，但还有很多是凭他们的实力制作出来的非常有用的软件。最后，不得不提到的是Node社区。虽然Node项目还非常年轻，但很少看到对一个项目如此狂热的社区。不管是新手，还是专家，大家都围绕着项目，使用并贡献自己的能力，致力于打造一个探索、支持、分享、听取建议的乐土。
- 模块打包代码是为了更方便的复用，意味着必须解决全局作用域冲突的问题，而ND的模块机制通过exports对象以及module.exports属性解决了全局作用于污染的问题，并且简化了代码的重用。
- CommonJS规范的意义。CJS规范为JS制定了一个美好的愿景，它主要解决了官方ECMAScript规范覆盖范畴小的问题。随着web2.0以及HTML5的兴起，对于JS语言规范本身而言，暴露出了缺乏模块系统、标准库少、缺乏标准接口、缺乏包管理系统等等缺陷，而CJS规范涵盖了包括模块、二进制、Buffer、字符集编码、IO、套接字等等一系列的范畴，从而使得NODE,W3C组织，CJS组织，ECMAScript共同构建成了一个繁荣的生态系统。

###1.1模块的创建
- 模块可以是一个文件，也可以是包含一个或者若干个文件的目录，如果模块是目录的形式，那么这个模块的入口必须是index.js。
- 创建方式1：exports对象属性定义，这些属性可以使任意类型的数据，比如字符串、对象和函数。
```node.js//创建一个加元和美元互换的模块，保存为currency.js文件
var canadianDollar = 0.91;
function roundTwoDecimals(amount){
	return Math.round(amount *100) / 100;
}
//分别创建两个函数，实现加元到美元，美元到加元的转换，为exports对象添加这两个属性
exports.canadianToUS = function(canadian){
	return roundTwoDecimals(canadian * canadianDollar);
}
exports.USToCanadian = function(us){
	return roundTwoDecimals(us / canadianDollar);
}
```

```node.js//以下创建一个js文件，在这个文件中需要引入我们之前创建的两个互换函数
var currency = require('./currency');
//这样我们就创立了一个currency对象
currency.canadianToUS(1000);
currency.USToCanadian(1000);
//通过调用这个对象的两个函数实现。
```
- 创建方式2：module.exports，这种方式不是创建包含两个函数的对象，而是通过构造函数创建一个模块的对象，因此封装的模块也要按照构造函数的格式进行封装。最终导出的结果来看，实际上exports就是module.exports的一个引用，因此如果按照构造函数的方式进行导出，就不能够给exports定义为其他的值。
```node.js
var Currency = function(canadianDollar){
	this.canadianDollar = canadianDollar;
}
Currency.prototype.roundTwoDecimals = function(amount){
	return Math.round(amount *100) / 100;
}

Currency.prototype.canadianToUS = function(canadian){
	return roundTwoDecimals(canadian * canadianDollar);
}
Currency.prototype.USToCanadian = function(us){
	return roundTwoDecimals(us / canadianDollar);
}

module.exports = exports = Currency;

//以下为在其他js文件中引用上述模块
var Currency = require('./currency');
var canadianDollar = 0.91;
var currency = new Currency(canadianDollar);
currency.canadianToUS(1000);
```
###1.2模块的寻址方式。
