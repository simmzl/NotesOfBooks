# NoteOfRhinoBook
> JS权威指南(犀牛书)读书笔记


> 创建这个项目的原因：秋招中发现自己的基础还是相对薄弱，希望能够完善自己的js基础，多过几遍犀牛书。之所以放在Github上，就当作给自己一个督促吧，每天都要看，并且及时更新笔记。

### 第一章 概述


```   
    var book = {
        name:"Rhino"
    }
```  
访问对象属性除了.还可以用[];

    book.name == book["name"];
数组和对象都可以包含另一个数组或者对象；

### 第二章 第三章 词法结构和类型、值、变量

js**区分大小写**，html不区分；如js构造函数名首字母大写等；
js支持Unicode,变量要求**以字母、$和_开头**，所有很多颜文字是可以用的2333：

    var ಠ_ಠ;
    var 눈_눈;
    var อิ_อิ;
js数据类型包括原始类型和对象类型；js中除了**数字、字符串、布尔、null、undefined外就是对象**，特殊的对象有数组、函数；null和undefined是无法拥有方法的值；
js解释器拥有自己的**内存管理机制，垃圾回收**；
js是**面向对象**的编程语言；
求余运算符%；
ES标准是不支持8进制数的，js中用16进制
    
    var a = 0xfff;
    isNaN(a); //false
0和-0只有在作为被除数时才不相同，1/0 === 1/-0 ;//正无穷大和负无穷大（很少用到）；
一个有趣的现象：任何使用二进制浮点数的编程语言在对十进制分数（0.1,0.01...）只能做到及其近似精确(有舍入误差)，amazing!
       
    var x = .3 - .2;
    var y = .2 - .1;
    x == y; //false;x = 0.09999999999999998 ,y = 0.1;
ES5中字符串当作**只读数组**，有**length属性，可使用索引,**调用方法返回的是个新字符串；
        
    var str = "hello,world"
    str.charAt(index);//取出第index的字符；
    str.substring(start,end);//返回第start到end的字符,不包括end
    str.slice(start,end);//同上
    str.slice(-3);//后三
    str.indexOf("o");//首次出现o的位置
    str.lastIndexOf("o");//最后出现o的位置
    str.indexOf("o",3);//在位置3及之后首次出现“o”的位置
    str.split(",")//以,分割(数组是splice(),还有arr.slice())
    str.replace("h","H");//替换
    str.toUpperCasr();//大写
    //还有一些正则表达式的方法等等
    ...
其中str.split()和数组的splice()和slice()有点绕，所以特意写了一篇[博客](http://blog.csdn.net/sinat_38752382/article/details/77717965)   