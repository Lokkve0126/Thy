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
    width:"100px";
    background: "red";
});

//$div.width();获取div的宽度 不包括边框
//$div.width("200px");设置div的宽度

//$div.eq(1).position();获取选中div的postion值top left
//$div.eq(1).position().left;获取left偏移量
//$div.eq(1).position().top;获取top偏移量

//$div.eq(1).offset().left;获取匹配元素在当前视口的相对偏移
//$div.eq(1).offset().top;

//$div.eq(1).html("<span>abc</span>");//插入html元素
//$div.eq(1).text("<span>abc</span>");//插入文本内容不能改变元素结构
//$input.eq(1).val("abc");//可以改变input输入框的值

```
##### 事件绑定以及事件[冒泡]委托
>在jquery中如果核心函数找到的元素不止一个，那么在添加事件时会遍历所有找到的元素，并绑定事件
```
*****事件绑定*****

//常用的事件绑定方法$div.eq(0).click(function() {
    alert("father");
});
$div.eq(1).click(function(e) {
    alert("son");
    //e.stopPropagation();
    //return false;
});
//当我们点击父元素时会弹出father,但点击子元素时会先弹出son，再弹出father这就是事件冒泡
//阻止事件冒泡的方法在回调函数中调用e.stopPropagation();  或者return false;来阻止事件冒泡

//上面绑定的方法只适用于已经写好事件的调用，下面可以自定义事件
$div.eq(1).on("click",function() {
    alert("son");
});
//on();第一个参数为事件 可空格隔开书写多个事件 也可以事件.命名空间来指定触发 例如:click.abc，第二参数可选一个选择器字符串用于过滤器的触发事件的选择器元素的后代 如果选择的< null或省略 当它到达选定的元素，事件总是触发 第三个参数可填false或者回调函数fn

*****事件移除*****
//$(ele).off();
//不传参数移除所有的事件
//一个参数移除指定类型的事件
//两个参数会移除所有指定类型的指定事件
//匿名函数无法移除

*****事件委托*****

//$("ul").eq(0).on("click","li,span",function() {
        alert($(this).text());
    });
    //给ul绑定click事件 并触发给子元素li和span

//$(ele).delegate();       jQuery 3.0中已弃用此方法，请用 on()代替。
//$("ul").delegate("li,span","click",function() {
        alert($(this).text());
    });
    //第一个参数为选择器字符串，用于过滤器触发事件的元素。
    //第二个参数为附加到元素的一个或多个事件。 由空格分隔多个事件值。必须是有效的事件。
    //第三个参数为回调函数

*****阻止默认事件*****

//a元素行为 填入了链接地址弹出窗口后会跳转
//$("a").click(function(e) {
        alert($(this).attr("href"));
        //return false;
        //e.preventDefault();
    });
    //第一种方法return false;
    //第二种方法e.preventDefault();
    //第三种ie ev.returnValue = false;

*****自动触发事件*****
//$(ele).trigger(事件,参数，参数...)
//trigger会自动触发事件并触发事件冒泡
//在触发自动触发a元素的点击事件时 要在a元素里的文字外套上元素span
再监听span的点击事件就可以触发并跳转 否则无效

//$(ele).triggerHandler(事件,参数，参数...)
//triggerHandler会自动触发事件但不会触发事件冒泡,不会执行浏览器默认动作,只触发jQuery对象集合中第一个元素的事件处理函数。
//传入的参数可以在回调函数中使用
```