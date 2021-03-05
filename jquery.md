# jquery学习笔记
***
##### 入口函数

```
$(document).ready(function() {
    //要写入的代码
});

//第二种写法
$(function () {

});
```
##### 原生JS跟JQUERY的加载模式不同：
>1.原生js需要等待DOM元素加载完毕，并且图片也加载完毕才会执行
>2.jquery同样会等到DOM元素加载完毕，但不会等待图片加载完毕
>3.原生js如果编写了多个入口函数，那么之后编写的会覆盖之前的
>4.jquery多个入口函数不会覆盖之前编写的

***
##### 解决jquery冲突：

```
jQuery.noConflict();//可以释放$符来解决冲突
var 自定义变量 = jQuery.noConflict();
var jq = jQuery.noConflict();//例
jq(document).ready(function() {});//通过自定义变量来调用jquery
```
##### forEach方法：
```
forEach();//原生js数组遍历方法 
var arry = [1,"abc","c",123];
arry.forEach(function(value,index,object) {
    value //当前遍历的元素
    index //当前遍历元素的索引
    object//当前遍历的数组
    console.log(value);
    console.log(index);
    console.log(object);
});

jquery里的Each方法：
例如:
<body>
            <div>abc</div>
            <div>bcd</div>
            <div>efg</div>
</body>
var $div = $("div");
$.each($div,function(index,value) {
    console.log(index,value);
    //可以遍历数组
    //同时可以遍历类数组
});
            //结果为
            0  <div>abc</div>
            1  <div>bcd</div>
            2  <div>efg</div>
```
##### map方法：
```
原生js map方法:
同上不能遍历类数组
arry.map(function(value,index,array) {
    console.log(value,index,array);
});

jquery map方法：
$.map(数组,回调函数);
$.map($div,function(value,index) {

});
```
>##### jquery中Each方法跟map方法的区别：
>Each 静态方法默认返回值为当前遍历谁就返回谁
>map 静态方法默认返回值是一个空数组
>Each 静态方法不支持在回调函数中对遍历的数组进行处理
>map 静态方法可以在回调函数中通过return对遍历的数组进行处理

##### trim方法：
```
//可以去除字符串两端的空格 并返回新的字符串
var str = "                abc   ";
var st = $.trim(str);
console.log(st);//结果为abc
```
##### 属性和属性节点
```
//attr方法
$div.eq(0).attr("class","abc");//选中div集合里的第一个div 并设置这个div的class为abc，若只有一个属性则为读取属性
$div.eq(0).removeAttr("class");//删除一个指定的属性

//prop方法
$div.eq(0).prop("class","abc");
$div.eq(0).removeProp("class");
//跟attr方法基本相同,区别在于prop()更多用于属性拥有true或者false的情况下使用，其他情况推荐使用attr()方法
```
##### jquery操作类 方法
```
//addClass(class|fn)
$div.eq(0).addClass("bcd efg");//此时第一个被选中的div的class变为"bcd efg" 添加多个class只需要空格隔开
//还可以传入一个回调函数 此函数必须返回一个或多个空格分隔的class名。接受两个参数，index参数为对象在这个集合中的索引值，class参数为这个对象原先的class属性值。

//removeClass(class|fn)
$div.eq(0).removeClass("bcd");//此时class bcd被删除 div的class为efg  同样删除多个需要用空格隔开书写
//回调函数同addClass()

//toggleClass(class|fn[,sw]);
$div.eq(0).toggleClass("abc");//如果存在class就添加（不存在）就删除
//通过设置switch来控制只删除或只添加
//$div.eq(0).toggleClass(class,switch);//switch == true||false
//$div.toggleClass(function(index,classNames,_switch) {
    if(_switch == true){
            return "abc-" + index;
        }else{
          return "abc-" + index + "ccc";
        }
    },true);// 这里的true，就是传递给函数callback的第三个参数_switch的值
//回调函数 1:用来返回在匹配的元素集合中的每个元素上用来切换的样式类名的一个函数。接收元素的索引位置和元素旧的样式类作为参数。2: 一个用来判断样式类添加还是移除的 boolean 值。
```
##### 操作HTML和CSS的方法
```
//$div.html("<div><span>123</span></div>");//设置
//$div.html();//获取

//$div.text("abcdefg");//设置文本内容
//$div.text();//获取文本内容

//$div.val("jquery");//设置value
//$div.val();//获取value

//$div.css("width","100px");//设置样式宽度为100px
//$div.css("width");//获取宽度
//多个属性设置的情况下可采用
//$div.css({
    width:100px;
    background: red;
});

//$div.width();获取div的宽度 不包括边框
```