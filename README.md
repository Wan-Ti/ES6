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

## 解构赋值

### 数组的解构赋值

从数组和对象中提取值，对变量进行赋值；只要等号两边的模式相同，左边的变量就会被赋予对应的值；
```
let [foo,[[bar],baz]] = [1,[[2],3]];
foo // 1
bar // 2
baz // 3

let [head,...tail] = [1,2,3,4];
head //1;
tail // [2,3,4]
```

如果解构不成功，变量的值就会等于undefin。,</br>
例如：</br>
```
let [foo] = [];
let [bar,foo] = [1]
```

还有一种不完全解构的情况：等号左边的模式只匹配一部分的等号右边的数组。这种情况下依旧可以解构成功

```
let [x,y] = [1,2,3];
x //1
y //2 

let [a,[b],d] = [1,[2,3],4];
a //1
b //2 
d //4
```
**解构成功的关键就是保证每个变量都能找到自己的值**

只要等号右边的值不是对象或数组，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。

### 对象的解构赋值

区别于数组解构赋值的是：数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值；</br>
其机制是：先找到同名属性，然后再赋给对应的变量。

```
let { bar, foo } = { foo: 'aaa', bar: 'bbb' };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: 'aaa', bar: 'bbb' };
baz // undefined
```

与数组一样，解构也可以用于嵌套结构的对象

```
let obj = {
  p:[
    'Hello',
    {y:'World'}
   ]
};

let {p: [ x, { y }] } = obj;
X //"Hello"
y // "World"

```
**注意：这时的P是模式，不是变量，不会被赋值。如果P也要作为变量赋值，可以写成这样：**
```
let obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};

let { p, p: [x, { y }] } = obj;
x // "Hello"
y // "World"
p // ["Hello", {y: "World"}]
```
### 字符串的解构赋值
```
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```
### 数值和布尔值的解构赋值

解构赋值时，如果等号右边是数值和布尔值，则会先转为对象

### 函数参数的解构赋值
```
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```
### 用途

1. 交换变量的值
2. 从函数返回多个值
   函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象中返回。通过解构赋值，取出这些值；
   ```
   // 返回一个数组

    function example() {
      return [1, 2, 3];
    }
    let [a, b, c] = example();

    // 返回一个对象

    function example() {
      return {
        foo: 1,
        bar: 2
      };
    }
    let { foo, bar } = example();
   ```
3. 函数参数的定义：将一组参数与变量名对应起来

**4. 提取JSON数据：**
```
let jsonData = {
  id:42,
  status:"ok",
  data:[867,5307]
 };
 let { id,status,data:number } = jsonData;
 console.log(id,status.number);
 //42,"ok",[867,5307]
```
5. 函数参数的默认值

**6.遍历Map结构**
```
const map = new Map();
map.set('first','Hello');
map.set('second','world');

for (let [key,value] of map) {
  console.log(key + "is" + value);
}

//first if hello 
//second if world
```
7. 输入模块的指定方法

加载模块时，需要指定输入哪些方法。
```
const { SourceMapCousumer,SourceNode } = require("source-map")
```

## 字符串的新增方法

indexOf():确定一个字符串是否包含在另一个字符串中。未找到返回-1；</br>
includes():是否找到了参数字符串，返回布尔值；</br>
repeat(n):将原字符串重复n次，返回一个新字符串。</br>
trim():删除字符串两端空格；trimStart():删除头部空格；trimEnd():删除尾部空格</br>
**matchAll():返回一个正则表达式在当前字符串的所有匹配**；</br>
replaceAll():替换所有匹配到的字符；</br>

## 数值的扩展

Number.inFinite()//参数是否是有限的</br>
Number.isNaN()//参数是否是数值</br>
Number.parseInt()//将该参数转为整型</br>

Math.trunc() //用于去除一个数的小数部分，返回整数部分；</br>
Math.sign() //判断为整数还是负数还是零;</br>
Math.imul() //两个参数带符号整数形式相乘；</br>

## 函数的扩展






















































































































































