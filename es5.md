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
