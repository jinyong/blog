# 数组
## 迭代方法
* every(): 对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true。
* filter(): 对数组中的每一项运行给定函数，返回该函数会返回true 的项组成的数组。
* forEach(): 对数组中的每一项运行给定函数。这个方法没有返回值。
* map(): 对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
* some(): 对数组中的每一项运行给定函数，如果该函数对任一项返回true，则返回true。

以上方法都不会修改数组中的包含的值。
```
var numbers = [1,2,3,4,5,4,3,2,1];
var everyResult = numbers.every(function(item, index, array){
  return (item > 2);
});
alert(everyResult); //false
var someResult = numbers.some(function(item, index, array){
  return (item > 2);
});
alert(someResult); //true
```
## 归并方法(递归？)
* reduce(): 第一项开始，逐个遍历到最后
* reduceRight(): 最后一项开始，向前遍历到第一项

这两个方法都接收两个参数：一个在每一项上调用的函数和（可选的）作为归并基础的初始值。

给reduce()和reduceRight()的函数接收4 个参数：前一个值、当前值、项的索引和数组对象。这
个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第
一个参数是数组的第一项，第二个参数就是数组的第二项。

求和例子：
```
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
  return prev + cur;
});
alert(sum); //15

var values = [1,2,3,4,5];
var sum = values.reduceRight(function(prev, cur, index, array){
return prev + cur;
});
alert(sum); //15
```
