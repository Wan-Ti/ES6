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

1. ES6允许函数的默认值直接设置参数；</br>
2. 参数默认值的位置一般书函数的尾参数；</br>
3. 函数的length属性返回值为没有指定默认值的参数个数；</br>

#### 参数默认值的应用

ES6引入reset参数，形式为```...变量名 ``` ，用于获取函数的多余参数。reset参数搭配的变量是一个数组。该变量将多余的参数放入数组中。

```
funciton add (...values){
  let sum = 0;
  for(let val of values){
  sum += val;
  }
  return sum
}

add(2,3,5) // 10
```
**reset参数之后不能再有其他参数**

#### name属性
函数的name属性返回该函数的函数名

#### 箭头函数

使用``` => ```定义函数

```
let f = () => 2; //如果代码块的语句大于1，用大括号括起来，并使用return语句返回

等价于

let f = function () {
  return 2
}
```

箭头函数没有自己的this，箭头函数内部的this永远指向最外层的this；</br>
因为没有自己的this所以无法使用.call,.bind,.apply改变this的指向；</br>

## 数组的扩展

### 扩展运算符

``` ... ```类似于rest参数的逆运算，将一个数组转为用逗号分隔的参数序列。主要用于函数调用，并且可以与正常的函数参数结合使用。

```
function add (x,y){
  return x + y;
}

const number = [4,30] 

add (...number)  //34
;
```
**应用**

#### 复制数组

```
const a1 = [2,3];

const a2 = [...a1];
```

#### 合并数组

const a1 = [1,2];
const a2 = [3,4];
const a3 = [5,6];

const a4 = [...a1,...a2,...a3]

console.log(a4) //[1,2,3,4,5,6]

#### Map和Set结构

```
let map = new Map([
  [1,'one'],
  [2,'two'],
  [3,'three']
]);
let add = [...map.key()];//[1,2,3]
```

### Array.from()

可以将两类对象转为真正的数组：类似数组的对象和可遍历的对象。

类似于数组的对象指的是：
```
let obj = {
  id:'1';
  name:'test'
};
let arr2 = Array.from(obj); //['1','test]
```

### Array.of()

用于将一组值转化为数组的形式；
```
Array.of(3,11,4);//[3,11,4]
```

### 数组实例的find()和findIndex()

find：用于找出第一个符合条件的数组成员。参数是一个回调函数。比如：
```
let a1 = [1,3,5,2,-4,-5]

a1.find((n) => n > 0) //1

a1.find((n) => n < 0) //-4 注意是第一个

```

### entires(),keys(),values()

遍历方式可用```for...of```进行遍历;</br>
唯一的区别是：keys()是对键名的遍历、values()是对键值的遍历、entries()是对键值对的遍历。

### Array.prototype.includes

返回一个布尔值，表示某个数组是否包含给定的值；

## 对象的扩展

### obj.assign()

用于对象的合并，将源对象的所有属性复制到目标对象；</br>
第一个参数是目标对象，其余参数为源对象；

```
const target = {a:1};

const obj1 = {b:2};
const obj2 = {c:3};

Object.assign(target,obj1,obj2)

console.log(target) // {a:1,b:2,c:3}
```




































































































































