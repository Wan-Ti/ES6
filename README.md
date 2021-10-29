# ES6
## let const var 的区别：
特点：let与const均存在不可重复声明变量与暂时性死区的特点；</br>
let 与 var：var命令会存在“变量提升”现象，即变量可以在声明之前使用，值为undefined;</br>
```
//var 的情况
console.log(foo);//输出undefind
var foo = 2;

//let的情况
console.log(bar);//报错ReferenceError
let bar = 2
```
本质区别：</br>
let用于声明变量，所声明的变量只在let命令所在的代码块内有效；</br>
const声明一个只读的常量。一旦声明，常量的值就不能改变,一旦声明就必须立即初始化，不能留到以后赋值；</br>
const实际上保证的并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但是对于复合型的数据(主要是对象和数组)，变量指向的内存地址，保存的只是一个指向实际数据的指针，const只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，就完全不能控制了；
![image](https://user-images.githubusercontent.com/47940363/139394873-11ee57c8-b79f-4811-b1c3-3dd30aba7cc6.png)


