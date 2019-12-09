# JavaScript

- javascript是一种弱类型编程语言。没有编译阶段，一个变量可以随意赋值。
    -   ```javascript
        var i = 100;
        i = fasle;
        i = "abc";
        i = new Object();
        i = 3.14;
        ```
    - undefined：在js中，如果一个变量没有手动赋值的时候，系统默认赋值undefined，是一个具体存在的值。  

- JS函数
    - 语法格式
        ```javascript
        function 函数名(形式参数列表){
            函数体;
        }
        ```
         ```javascript
        函数名 = function(形式参数列表){
            函数体;
        }
        ```
    - JS中的函数不需要制定返回值类型，返回什么类型都行。
    - 函数在调用的时候，参数的类型没有显示，并且参数的个数也没有限制，JS就是这么随意。
    - JS中函数名字不能重名，重名的时候，后声明的函数会覆盖之前声明的同名函数。  

- 全局变量和局部变量
    - 全局变量
        - 在函数体之外声明的变量属于全局变量，全局变量的生命周期：浏览器打开时声明，浏览器关闭时销毁，尽量少用。会一直存在于浏览器的内存中，耗费内存空间。尽量使用局部变量。
    - 局部变量
        - 在函数体中声明的变量，包括一个函数的形参都属于局部变量。
    - 一个变量在声明的时候不加上`var`的话，不管在哪里声明，都是全局变量。  
- JS数据类型
    - Number：整数，小数，整数，负数，不是数字，无穷大都属于Number类型。 
        ```javascript
        var a = 100;
        var b = "中国人";

        alert(a / b);
        // 最后结果应该是一个数字，所以会弹出NaN。

        var a = "abc";
        var b = 10;

        alert(a / b);
        //最后结果是一个字符串"abc10"

        //JavaScript会优先进行前面的数据类型的运算
        ```
        - `Math.ceil()`作用：向上取整。  

- JS中如何定义类，如何new对象？  
    - 定义类的语法：
        ```javascript
        function 函数名(形式参数列表){
            函数体;
        }
        ```
        ```javascript
        函数名 = function(形式参数列表){
            函数体;
        }
        ```
    - 创建对象的语法：
        ```javascript
        new 构造方法名(实参);

        //不写new，就是直接把这个方法当成一个普通的函数来调用；写new就相当于创建一个新对象。
        ```

- JS中类的定义，同时又是一个构造函数的定义，换句话说，类的定义和构造函数的定义是放在一起完成的。
    ```javascript
    function User(a, b, c){ //abc是形参，属于局部变量
    //声明属性（this表示当前对象）
    //User类中有三个属性：sno/sname/sage
        this.sno = a;
        this.sname = b;
        this.sage = c;
    // 函数
        this.getNo = function(){
            return this.sno;
        }
    }
    //创建对象
    var u1 = new User(111, "zhangsan", 30);
    //访问对象的属性
    alert(u1.sno);
    alert(u1.sname);
    alert(u1.sage);
    //调用get函数
    var no = u1.getNo;
    alert(no);

    //可以通过prototype这个属性来给类动态扩展属性以及函数
    User.prototype.getName = funciton(){
        return this.sname;
    }
    var name = u1.getName;
    alert(name);
    ```
- JS常用事件 
    1.  blur 失去焦点
    2. change 下拉列表选中项改变，或者文本内容改变
    3. click 鼠标单击
    4. ablclick 鼠标双击
    5. foucus 获得焦点
    6. keydown 键盘按下
    7. keyup 键盘弹起
    8. load 页面加载完毕
    9. mousedown 鼠标按下
    10. mouseover 鼠标经过
    11. mousemove 鼠标移动
    12. mouseout 鼠标离开
    13. mouseup 鼠标弹起
    14. reset 表单重置
    15. select 文本被选定
    16. submit 表单提交

- 回调函数  
    - 方法一
        ```javascript
        function sayHello(){
            alert("Hello JS!");
        }
        //直接在标签中使用事件句柄
        <input type = "button" value = "hello" onclick = "sayHello()">
        ```
    - 方法二
        ```javascript
        //纯JS代码完成事件的注册
        function soSome(){
            alert("Do some!");
        }
         <input type = "button" value = "hello" id = "mybtn">
         //第一步：先获取这个按钮对象
         var btnObj = document.getElementById("mybtn");
         //第二步：给按钮对象onclick属性赋值
         btnObj.onclick = doSome;//不能加小括号
        ```
    - 匿名函数
        ```javascript
        var test = document.getElementById("test");
        test.onclick = function(){
            alert("test!");
        }

        document.getElementById("test").test.onclick = function(){
            alert("test!");
        }
        ```
- JSON - Javascript Object Notation JS对象表示法
    ```javascript
    // 创建一个对象
    var obj = {"name":"monkey king","age":18,"gender":"male"};
    ```
    - JS中的对象只有JS认识，其他语言都不认识。如果需要把这个对象传给后段服务器语言JAVA，该怎么办呢？
    ```javascript
    var obj = '{"name":"monkey king","age":18,"gender":"male"}';
    ```
    - 前后各加一个单引号 '
    - JSON就是一个特殊格式的字符串，这个字符串可以被任意的语言所识别。并且可以转换为任意语言中的对象，JSON在开发中主要用来数据的交互。 
    - JSON和JS的对象格式一样，只不过JSON字符串中的属性名必须加双引号。
        - JSON分类：  
            1. 对象{ }
            2. 数组[ ]
        - JSON中允许的值
            1. 字符串
            2. 数值
            3. 布尔值
            4. null
            5. 对象
            6. 数组
        - JSON -> JS对象  
            - JSON.parse()
                - 可以将JSON字符串转换为JS对象 

