---
title: jquery对象和dom对象
date: 2019-03-12 16:26:58
tags:
---

# jquery对象和dom对象

jQuery实际上可以说是一个大的类——javascript实现的类。以一个简单的模型来说，如下：
```js
;(function(window, undefined){
         
    window.$ = window.jQuery = jQuery;
     
    // 定义jQuery类
    function jQuery(selector, content){
        content = content || document;
        var eles = content.querySelectorAll(selector);
        var len = eles.length;
         
        // 给jQuery对象添加长度属性
        this.length = len;
         
        // 方便获取dom对象，获取实例：jQuery('#id')[0];
        for(var i = 0; i < len; i++){
            this[i] = eles[i];
        }
    }
     
    // 扩展原型
    jQuery.prototype = {
        // 构造函数
        constructor : jQuery,
         
        // 根据索引获取dom对象
        get : function(index){
            return this[index];
        }
    }
})(window);
```
上面是一段jquery类的模拟代码。  
下面是另一段jquery模拟代码。
```js
window.jQuery = function(selectorOrNodeOrWhatever){
  if(! (this instanceof jQuery) ){
    return new jQuery(selectorOrNodeOrWhatever)
  }
  this[0] = firstNode
  this[1] = sencondNode
  this.length = 2
}
window.jQuery.prototype = {
  addClass: function(){},
  removeClass: function(){},
  text: function(){},
  html: function(){},
  //其他 API 都是些简单的单词而已
}

window.jQuery.ajax = function({url, method, data}){
  //调用 XMLHttpRequest 发请求
  //返回一个 Promise
}
window.$ = window.jQuery

//作者：方应杭
//链接：https://www.zhihu.com/question/267180315/answer/329684448
//“jquery本质就是dom api和ajax的封装”
```
### jQuery对象转dom对象

```js
var $li = $(“li”);
//第一种方法（推荐使用）
$li[0]
//第二种方法
$li.get(0)
//其实jQuery对象转DOM对象的实质就是取出jQuery对象中封装的DOM对象。
```
### dom对象转jquery对象
```js
var $obj = $(domObj);
// $(document).ready(function(){});就是典型的DOM对象转jQuery对象
```
### example
对于如下的html代码
```html
<table class="table table-bordered" id="participatorsStatisticsTable">
    <thead>
    <tr>
        <th>高级<span class="reqMark">*</span></th>
        <th>中级<span class="reqMark">*</span></th>
        <th>初级<span class="reqMark">*</span></th>
        <th>博士后<span class="reqMark">*</span></th>
        <th>博士<span class="reqMark">*</span></th>
        <th>硕士<span class="reqMark">*</span></th>
        <th>总人数<span class="reqMark">*</span></th>
        <th>参加单位数<span class="reqMark">*</span></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td><input type="text" class="form-control validate[required,custom[onlyNumberSp]]" id="advancedNum"></td>
        <td><input type="text" class="form-control validate[required,custom[onlyNumberSp]]" id="intermediateNum"></td>
        <td><input type="text" class="form-control validate[required,custom[onlyNumberSp]]" id="elementaryNum"></td>
        <td><input type="text" class="form-control validate[required,custom[onlyNumberSp]]" id="postdoctorsNum"></td>
        <td><input type="text" class="form-control validate[required,custom[onlyNumberSp]]" id="doctorsNum"></td>
        <td><input type="text" class="form-control validate[required,custom[onlyNumberSp]]" id="mastersNum"></td>
        <td><input type="text" class="form-control validate[required,custom[onlyNumberSp]]" id="headcount"></td>
        <td><input type="text" class="form-control validate[required,custom[onlyNumberSp]]" id="departmentNum"></td>
    </tr>
    </tbody>
</table>
```
首先执行：
```js
var $tr = $('#participatorsTable').find('tbody > tr');
var $inputs = $tr.find('input')
```
此时将``$input``在console打出，得到：  
![image](https://note.youdao.com/yws/api/personal/file/5984DF95AEAA4ABC80CFB3E1350C6ADD?method=download&shareKey=57b429163dca62249af13f56cdaebd81)  
可以看出此时可通过执行``$inputs[0]``至``$inputs[4]``得到``tbody``中的五个``input``元素，对象类型为dom对象。  
这一点可以通过执行下面的命令来得到结果。
```js
$inputs[3].constructor; //HTMLInputElement()
```
