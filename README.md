# NoteOfRhinoBook
> JS权威指南(犀牛书)读书笔记


> 创建这个项目的原因：秋招中发现自己的基础还是相对薄弱，希望能够完善自己的js基础，多过几遍犀牛书。之所以放在Github上，就当作给自己一个督促吧，每天都要看，并且及时更新笔记。



> 2017/08/30更
## 第一章 概述


```   
    var book = {
        name:"Rhino"
    }
```  
访问对象属性除了.还可以用[];

    book.name == book["name"];
数组和对象都可以包含另一个数组或者对象；

## 第二章 第三章 词法结构和类型、值、变量

1. js**区分大小写**，html不区分；如js构造函数名首字母大写等;
2. js支持Unicode,变量要求**以字母、$和_开头**，所有很多颜文字是可以用的2333：

    ```javascript
             var ಠ_ಠ;
             var 눈_눈;
             var อิ_อิ;
     ```
    
3. js数据类型包括原始类型和对象类型；js中除了**数字、字符串、布尔、null、undefined外就是对象**，特殊的对象有数组、函数；null和undefined是无法拥有方法的值；
4. js解释器拥有自己的**内存管理机制，垃圾回收**；
5. js是**面向对象**的编程语言；
6. 求余运算符%；
7. ES标准是不支持8进制数的，js中用16进制
    ```javascript
        var a = 0xfff;
        isNaN(a); //false
    ```
8. 0和-0只有在作为被除数时才不相同，1/0 === 1/-0 ;//正无穷大和负无穷大（很少用到）；
9. 一个有趣的现象：任何使用二进制浮点数的编程语言在对十进制分数（0.1,0.01...）只能做到及其近似精确(有舍入误差)，amazing!
    ```javascript   
        var x = .3 - .2;
        var y = .2 - .1;
        x == y; //false;x = 0.09999999999999998 ,y = 0.1;
    ```
10. ES5中字符串当作**只读数组**，有**length属性，可使用索引,**调用方法返回的是个新字符串；
        
    ```javascript
        var str = "hello,world";
        str.charAt(index);//取出第index的字符；
        str.substring(start,end);//返回第start到end的字符,不包括end
        str.slice(start,end);//同上
        str.slice(-3);//后三
        str.indexOf("o");//首次出现o的位置
        str.lastIndexOf("o");//最后出现o的位置
        str.indexOf("o",3);//在位置3及之后首次出现“o”的位置
        str.split(",");//以,分割(数组是splice(),还有arr.slice())
        str.replace("h","H");//替换
        str.toUpperCasr();//大写
        //还有一些正则表达式的方法等等
    ```
其中str.split()和数组的splice()和slice()有点绕，所以特意写了一篇[博客](http://blog.csdn.net/sinat_38752382/article/details/77717965)   

> 201/08/31更
### 3.9 变量声明
1. js是动态语言（Python/Ruby），即不给任何变量指定数据类型，相对的，C/C++/C#/JAVA则是静态语言；
2. 在严格模式下给一个未声明的变量赋值会报错，非严格则会提升为全局变量；
3. 只声明不赋值变量数据类型为**undefined**；
### 3.10 变量作用域scope
1. 在函数体内，局量（局部变量）的优先级高于同名的全量。即若在函数内声明一个和全量重名的局量或者和全量重名的参数，则全量在函数体内会被其覆盖；
    ```javascript
        //局部变量
        var a ="global";
        function checkScope(){
            var a="local";
            return a;
        }
        console.log(checkScope());//local
        console.log(a);//global
        
        //参数
        var b ="global";
        function checkScope(b){
            b="local";
            return b;
        }
        console.log(checkScope());//local
    ```
2. **函数作用域和声明提前**
    1. 在C语言中，每一个花括号内的代码都有各自的作用域，叫做块级作用域，而**js是没有块级作用域的**（block scope）;
    2. **js使用的是函数作用域**（function scope）：变量在声明他们的函数体以及这个函数嵌套的函数体内都是有定义的；
    3. **声明提前**：js函数里声明的所有变量（**但不涉及赋值**）都被**提前到了函数体的顶部**！
        ```javascript
           var scope = "global";
           function f() {
               console.log(scope);//undefined(不是global，也不是local)
               var scope = "local"; //该声明会提前到函数体顶但是没有把赋值提前，只有程序执行到该语句才会被赋值
               console.log(scope); //此时局部变量scope才被赋值为local
           }
           //所以可以将变量声明放在函数体顶部，然后用到时再赋值，这是个不错的编程习惯！
        ```
        
    4. JS的全局变量相当于全局对象的属性。使用var 声明时无法删除（delete）;
        ```javascript
            var a = 1;
            b = 2;
            this.c = 3;
            delete a;
            delete b;
            delete c;
            console.log(a);//1
            console.log(b);//未定义
            console.log(c);//未定义
        ```
    5. **作用域链** 
        1. 每一段js代码（全局代码或者函数）都有一个与之关联的作用域链，这个作用域链是一个**对象列表或链表**；
        2. “**变量解析**”过程：查找一个变量x的值时，先**从链的第一个对象中查找**，如果该对象有叫x这个属性的话，则使用这个属性的值，
        否则查找链上的下一个对象，以此类推，直到找到，否则抛出一个引用错误（Reference Error）的异常。
        3. 所以对于最顶层代码，链上只有一个全局对象；在全局对象包含的一个没有嵌套的函数体内，其链上第一个是定义该函数参数和局部变量的对象，第二个是全局对象，依次类推；
        4. 定义一个函数时，他的**作用域已被决定**。调用该函数时，程序会创建一个新的对象来存储该函数的局部变量，并将这个对象添加到该函数本来的作用域链上。（这里意思我觉得应该就是复制原来的作用域链给自己）
        5. 对于嵌套函数来说，每次其外部函数被调用时，内部函数会被重新定义了一遍，因为调用时创建了一个新的对象，所以这时候嵌套函数的作用域链也会跟着改变。
> 2017/9/1        
### 8.9 闭包
1. 理解了作用域链，闭包也就好理解多了；
2. 当内部函数在定义它的作用域的外部被引用时,就创建了该内部函数的闭包,如果内部函数引用了位于外部函数的变量,当外部函数调用完毕后,
这些变量在内存不会被释放,因为闭包需要它们，涉及js垃圾回收机制；
    ```javascript
    function outerFun(){
         var a=0;
         function innerFun()
         {
          a++;
          alert(a);
         }
         return innerFun;  //注意这里
        }
        var obj=outerFun();
        obj();  //结果为1
        obj();  //结果为2
        var obj2=outerFun();
        obj2();//结果为1
        obj2();//结果为2
    ```
3. 闭包着实不好理解.....前面我把闭包想简单了....在参考了阮一峰等大神的博客之后写了一篇[博客]()作为整理思路，理解闭包之后js的Garbage Collection好理解多了...在博客最后也写到了GC；
> 2017/9/2
## 第三章下 类型、值、变量
    ```javascript
        typeof(null);//object
        typeof(undefined);//undefined
        undefined == null;//true
        undefined === null;//false
    ```
### 全局变量
1. js解释器启动时（浏览器加载页面时），创建一个新的全局对象，定义一些初始属性：
    1. 全局属性：undefined、NaN;
    2. 全局函数：parseInt(),eval()等；
    3. 构造函数：Date(),RegExp(),String()，Array()等；
    4. 全局对象：Math,JSON
2. 在代码最顶层可使用this引用全局对象；
3. 在浏览器中window充当全局对象；
### 一些对象,诸如包装对象、对象比较
1. 
    ```javascript
    var s ="test";
    s.indexOf("e");//1
    s.len=4;
    var a =s.len;
    console.log(a);//undefined
    ```
如上代码，s.indexOf()可以调用Sting对象的方法，所以s是个临时对象，但s.len的值并没有赋给a;
这是因为字符串s引用Sting对象的方法indexOf()时，会创建一个临时对象，**引用完之后就被GC回收**，此时不是对象就不能创建一个新的属性len；a自然就未定义；
**所以，包装对象就是存取字符串、数字、布尔的属性时创建的临时对象；**
2. 
    ```javascript
    var s = new String("ab");
    s.len=2;
    var a =s.len;
    console.log(a);//2
    ```
通过构造函数显式创建包装函数，可以实现为其添加属性（这是一般正常的用法~~~）
3. 原始值（undefined,null,布尔，数字，字符串）不可更改，对象中的值（数组，函数）可以更改；
4. 两个属性和属性值都相等的对象也是**不相等**的：

    ```javascript
    var a={x:1},b={x:1};
    a===b;//false,两个=也不相等
    var c = [1,2,3],d=[1,2,3];
    c===d;//false
    ```
### 类型转换
1. 转为数字和布尔值：

    ```
    undefined -> NaN/false
    null -> 0/false
    "" ->0/false
    "1.2" ->1.2/true
    "one" ->NaN/true
    0/-0 ->0/false
    NaN ->false
    [] ->0/false
    (除了0、-0、NaN的数字都是true)
    空数组、空对象、空函数都是true
    ```
    
使用“==”判断时，会自动转化类型，用“===”则不会；
2. 显示转换，调用构造函数如Number("3");
3. toString(进制)和toFixed(0/1/2/3/...)(保留n位小数)
    ```javascript
        var n=17;
        var turn = n.toString(空/2/8/16);//不同进制
    ```
## 第四章 表达式和运算符
都是一些常见的东西...
1. **in运算符**测试属性是否存在，左侧为字符串或者可以转换为字符串，右侧为对象；(这个就有点像for-in语句了)
    ```javascript
    var obj = {x:1,y:2};
    "x" in obj;//true
    "toString" in obj;//true(因为obj继承了toString())
    var arr=[1,2,3];
    "1" in arr;//true
    1 in arr;//true
    ```
2. instanceof;左侧为一个对象（否则返回false），右侧为标识对象的类（**涉及原型链**）；
    ```javascript
     var d = new Date;
    d instanceof Date;//true
    d instanceof Object;//true
    //(数组同理)
    ```
3. **&& 新理解！！！**
&&原来除了我之前一直以为的对布尔值或者转换后的布尔值进行与操作外，还有第三层骚操作！！！
    1. &&语句会先运行左操作数，如果左操作数返回结果为false，则其不会再运行右操作数；
    2. 左为真，则其会运行右操作数并将右操作数返回结果作为整个表达式的结果！
    
        ```javascript
            if(a == b)stop();
            //等价于
            (a==b) && stop();
        ```
    3. 很骚！
4. || 计算第一个操作数，真则返回该结果；否则运算第二个；都为假则返回假值；
5. eval();