---
title: Javascript系统复习：数组专栏
date: 2016-08-21
tags: ["javascript"]
---
## 前言:
数组！一个出镜率非常高的数据结构～～      
定义： 数组是值的有序集合，每个值叫做元素，每个元素在数组中都有数字位置编号，也就是索引。JS中数组是弱类型的， 即数组中可以含有不同类型的元素。数组元素甚至可以是对象或其它数组。     

<!-- more --> 
    
🌰：   
   
```js
var arr = [1, true, null, undefined, {x:1}, [1,2,3]];
```

看看，是不是啥都有？^_^    
   
## 数组的创建
直接看代码：    
   
### 字面量形式创建数组：    
   
```js
var bat = ["ali", "tecent", "baidu"];
var students = [{name:"xiaoming", age:27}];
```

我们可以用逗号来留空：    
   
```js
var commasArr1 = [1, , 2]; // 1, undefined, 2
var commasArr2 = [,,]; // 允许最后有个多余的逗号，它会被忽略
```
    
数组的长度是有限制的，最小的是0，最大的是2^23-1(4294967295)。   
   
### Array构造函数创建数组：    
   
```js
var arr = new Array();
var arr100 = new Array(100);
var arrLikesLiteral = new Array(true, false, null, 1, 2, "hi");
```
如果你啥都不传，就是一个空数组。   
如果你传入的仅仅是一个数字，那么它就是这个数组的长度。   
如果你传入了多个参数，那么它们就成为了数组的内容。    
    
## 数组元素的读写
   
```js
var arr = [1,2,3,4,5];
arr[1]; //2
arr.length; //5
```

楼上我们用索引访问数组元素，用length属性访问它的长度。   
 
    
## 数组元素的增删
   
我们可以动态地给数组赋值来增加它的元素：   

```js
arr[5] = 6;
arr.length;// 6
```
这样它就多了一个元素，变成了长度为6的数组。    
  
我们还可以用push方法在尾部增加元素:   
   
```js
var arr = [1,2];
arr.push(3);
arr; // [1,2,3];
```
   
用unshift方法在头部增加元素:    
   
```js
arr.unshift(0);
arr; //[0,1,2,3,4]
```


    
我们还可以删除数组元素，用delete运算符：    
   
```js
var arr = [1,2,3,4,5];
delete arr[0];
arr[0]; //undefined
arr.length; // 5
```
数组长度是不变的，只是把第一个元素变成了undefined而已～～～  
缩减数组长度，最简单粗暴就是修改length:    
   
```js
var arr = [1,2,3,4];
arr.length -= 1;
arr; // [1,2,3], 4 is removed
```

pop方法删除尾部元素:   
   
```js
var arr = [1,2,3,4];
arr.pop();
arr; [1,2,3];
```

shift方法删除头部元素:   
   
```js
var arr = [1,2,3,4];
arr.shift();
arr; // [2,3,4];
```
   
## 数组迭代
到数组方法那一块我们会介绍forEach方法，这里我们主要介绍原始策略:    

最容易想到的数组迭代方式就是for循环：    
   
```js
var i = 0, n = 10;
var arr = [1, 2, 3, 4, 5];
for(; i < n; i++) {
    console.log(arr[i]);
}
```

或者for in 循环：   
   
```js
for(i in arr) {
    console.log(arr[i]);
}
```

注意，如果用for in ，数组也是对象，它也有原型，for in会把原型链上的东西也输出。   
还有个坑， for in是无序的，所以如果想要有序输出，还是for吧！   
    
## 数组的两种特殊形式
### 二维数组
比如这样的：   
   
```js
var arr = [[0, 1], [2, 3], [4, 5]];
```
数组套数组，里面的数组里面不再套数组，这样的就是二维数组。    
   
### 稀疏数组
***稀疏数组就是并不含有从0开始的连续索引的数组，一般length属性值比实际元素个数大***
      
举个例子：    
   
```js
var arr1 = [undefined];
var arr2 = new Array(1);
0 in arr1;// true
0 in arr2;// false
arr1.length = 100;
arr1[99] = 123;
99 in arr1;// true
98 in arr1;// false
```
第一个中，我们的arr1是有元素的，只不过元素的值是undefined。    
第二个中，虽然创建了一个长度为1的数组，但是没给它赋值，所以它里面0索引是不存在的。   
   
稀疏数组在实际的应用中其实不是很常见，我们一般用in来遍历它，来减少开销。    
   
## Javascript数组方法
楼上我们是不是调用了很多数组的方法，数组的方法从哪里来？   
当然是来自原型链啦！   
数组对象的原型就是Array.prototype~~   
    
让我们来一个全家桶吧！  
  
### join方法,将数组转换为字符串：   

```js
var arr = [1, 2, 3];
arr.join(); // "1,2,3"
arr.join("_"); // "1_2_3"
```

不写参数，默认把数组元素以逗号隔开。    
   
### reverse方法  将数组逆序
```js
var arr = [1,2,3];
arr.reverse();
arr; //[3,2,1];
```

### sort方法 排序
   
```js
var arr = ["a", "c", "d", "b"];
arr.sort(); // ["a", "b", "c", "d"]
```

sort方法对什么都是按照字符串的排序方法排序的。    
因此排序数字时：   
   
```js
arr = [13, 24, 51, 3];
arr.sort();
arr; // [13, 23, 3, 51]
```
非常尴尬     
因此我们可以自定义sort:    
   
```js
arr.sort(function(a, b){
  return a-b;
});
arr; [3, 13, 24, 51]
```
自定义sort函数，我们在sort中传入我们希望的排序规则，即比较函数。    
在排序的时候，任两个元素都会被传入比较函数中，如果它的返回值是负数，那么不改变原有的顺序，如果它的返回值是正数，那么交换两个数的顺序～～。    
   
### concat方法 合并
```js
var arr = [1, 2, 3];
arr.concat(4, 5); // [1,2,3,4,5]
arr; //[1, 2, 3]
```
注意concat和楼上那些方法的不同在于concat并不改变数组本身。   
concat可以直接传入数组，这样传入的数组会被拉平(只会拉平一次)。  
   
```js
arr.concat([10, 11], 13);
arr; // [1, 2, 3, 10, 11, 13]
```
   
### slice 方法 返回数组片段
```js
var arr = [1, 2, 3, 4, 5];
arr.slice(1, 3); // [2,3]
arr.slice(1); // [2,3,4,5]
arr.slice(1, -1); // [2,3,4]
arr.slice(-4, -3); // [2]
```
第一个参数是截取开始的索引，第二个参数的截取结束的后一个索引。   
也就是左闭右开区间。   如果第二个参数省略，默认输出后面的所有。
－1代表最后一个索引，－2代表倒数第二个索引，以此类推。  
注意，原数组仍然不被修改。 
   
### splice方法 数组删除和拼接
🌰：    
   
```js
var arr = [1, 2, 3, 4, 5];
arr.splice(2); //returns [3,4,5]
arr; // [1,2] 原数组被修改

arr = [1,2,3,4,5];
arr.splice(2,2); // return [3,4] 
arr; // [1,2,5]

arr = [1,2,3,4,5];
arr.splice(1,1,"a","b"); //returns [2]
arr; // [1,"a","b",3,4,5]
```

splice 第一个参数是说你从哪里开始删除，第二个位置是说你要指定删除几个，第三个位置是说你要往删除位置上填补什么东西。   
后两个都可以省略。   
第二第三一起省略的时候，就是删除从第一个参数开始包括到最后一个元素全都删除。   
第三个元素省略的时候，就是删除指定位置指定个数的元素。   
三个参数全都有，就是说先删除后填补～～    
   
***注意splice方法是修改原数组的***   
    
### forEach方法
这就是数组迭代那一块我说的forEach方法。   
   
🌰：   
   
```js
var arr = [1,2,3,4,5];
arr.forEach(function(x, index, a) {
    console.log(x + "|" + index + "|" + (a===arr));
});
// 1|0|true
// 2|1|true
// 3|2|true
// 4|3|true
// 5|4|true
```

forEach中的函数有三个参数，第一个是元素的值，第二个是元素的索引，最后的a是指向数组本身。    
### map方法
对数组做映射的方法。   
🌰：   
   
```js
var arr = [1,2,3];
arr.map(function(x) {
  return x+10;
});// [11,12,13]
arr; // [1,2,3]
```

map会在遍历每一个元素的时候去调用。   
map的返回值会组成一个新数组，不会修改原来的数组。   
   
### filter方法 数组过滤
🌰：   
   
```js
var arr= [1,2,3,4,5,6,7,8,9,10];
arr.filter(function(x, index){
  return index % 3 === 0 || x >= 8;
}); // returns [1, 4, 7, 8, 9, 10]
```

filter中传入一个过滤函数，这个过滤函数有两个参数，分别是元素的值和元素的索引。   
filter的返回值会组成一个新数组，不会修改原来的数组。 
    
### every和some方法
这两个方法都是返回布尔值，所以是对数组元素进行判断的方法。   
every在****每个数组元素**作为参数传入它的判断函数中都返回true的情况下返回true。   
some在****存在一个数组元素**作为参数传入它的判断函数中返回true的情况下返回true。   

🌰：   
   
```js
var arr = [1, 2, 3, 4, 5];
arr.every(function(x) {
    return x < 10;
}); //true
arr.every(function(x) {
    return x < 3;
}); // false
```

```js
var arr = [1,2,3,4,5];
arr.some(function(x) {
  return x === 3;
}); // true
arr.some(function(x) {
    return x === 100;
}); // false
```

### reduce和reduceRight方法
是对数组元素的两两操作～～   
reduce🌰：  

```js
var arr = [1,2,3];
var sum = arr.reduce(function(x, y){
  return x + y;
}); // 6
arr; // [1,2,3]
```
其中x第一次调用的时候是第一个元素，后面调用的时候都是前一次reduce的值。   
   
reduceRight和reduce相似，只不过是从右边开始遍历的。  
   
### indexOf和lastIndexOf
查找索引的方法。   
   
🌰：   
   
```js
var arr = [1,2,3,4,5];
arr.indexOf(2); //1
arr.indexOf(99); //-1
arr.indexOf(1,1); //4
arr.indexOf(1,-3); //4
arr.indexOf(2,-1); //3
arr.lastIndexOf(2,-2); //3
arr.lastIndexOf(2,-3); //1
```
第一个参数是我们希望找到的那个值。  
如果只传入第一个参数，就在整个数组范围内查找。   
注意返回值在元素存在的情况下是非负整数，在元素不存在的情况下是-1。   
第二个参数是开始查找的元素的索引。比如我们想要从第二个元素开始查找，就把第二个参数设为1.   
lastIndexOf大同小异，只是说它查找的顺序从右往左，而楼上是从左往右。   
   
### Array.isArray方法
这个是用来判断一个东西是不是数组的。   
判断是否为数组有很多方法:   
   
```js
Array.isArray([]); //true
[] instanceof Array; //true
({}).toString.apply([]) === "[object Array]"; // true
[].constructor === Array; "true"
```
   
## 总结（补充数组和字符串）
今天复习了数组的概念和方法。   
其实数组也是对象的一种，只是它自身有一些特殊的机制，是特殊的对象。   
明显的不同是：   
- 数组自动更新length
- 按索引访问数组元素的速度比按属性访问对象属性的速度快
- 数组对象继承了Array.prototype上的大量数组操作方法
   
另外，补充一下，字符串和数组也是很有一些关系的。   
  
🌰：    
   
```js
var str = "hello world";
str.charAt(0); //"h"
str[1]; // e

Array.prototype.join.call(str, "_");
// "h_e_l_l_o__w_o_r_l_d"
```
字符串是一种类数组，它有很多类似于数组的操作。   
它不是数组，只是相似～～    
但是正因为相似，它可以直接使用数组上的一些方法。   





   







   

    

   


