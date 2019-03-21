---
title: js中批量删除数组中某几项可能的错误
date: 2019-03-12 18:50:01
tags:
---
有数组a和b，现在要删除a数组中index值存在与b数组中的项。
```js
let a = [0, 1, 2, 3, 4, 5, 6, 7];
let b = [1, 2, 3, 4, 5, 6];
```
使用``Array.prototype.splice()``：
```js
let a = [0, 1, 2, 3, 4, 5, 6, 7];
let b = [1, 2, 3, 4, 5, 6];
b.forEach((value, index) => {
    a.splice(value, 1);
});
console.log(a);
```
然而得到的结果是：``[ 0, 2, 4, 6 ]``。  
原因是，当删除第一个项后，原来的第二项变成了第一项，以此类推，仅删除了部分项。  
因此修改为：
```js
let a = [0, 1, 2, 3, 4, 5, 6, 7];
let b = [1, 2, 3, 4, 5, 6];
b.forEach((value, index) => {
    a.splice(value - index, 1);
});
console.log(a);
```
得到正确结果：``[0, 7]``