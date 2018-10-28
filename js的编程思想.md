# 声明：学习编程语言最好的方式就是通过实例学习
　　　　　　　
## 下面是我在博客上看到的一道js面试题，可以说非常经典，下面会以最简单的方式让你理解
题目：

```bash
function Foo() {
    getName = function () { alert (1); };
    return this;
}
Foo.getName = function () { alert (2);};
Foo.prototype.getName = function () { alert (3);};
var getName = function () { alert (4);};
function getName() { alert (5);}
 
//请写出以下输出结果：
Foo.getName();			//第一题　　答案:2
getName();			//第二题　　答案:4
Foo().getName();　　　　　　　　　　　　　　　　//第三题　　答案:1
getName();                      //第四题　　答案:1
new Foo.getName();              //第五题　　答案:2
new Foo().getName();            //第六题　　答案:3
new new Foo().getName();        //第七题　　答案:3

```

## 解释
### 第一题　Foo.getName();
(1).这一题涉及静态属性

```bash
    例如：
　　　　　　　　function A(){}
        A.name = "我是A的静态属性name";
        console.log((A.name);
```

事实上执行的是:
              Foo.getName = function () { alert (2);};

### 第二题　getName();
(1).这一题涉及函数声明和函数表达式(
    共同点：1.两者都会提升到作用域开始部分
　　不同点：1.函数声明提升的是整个函数对象,也就是说你即使在函数定义之前调用这个函数,这个函数也会被执行
　　　　　　　函数声明:function A(){}
　　　　　　　　　　　2.函数表达式提升之后，表达式值为undefined,也就是说你需要在函数定义之后调用这个函数表达式
　　　　　　　函数表达式:var A=function(){}
           
　　　　　　　　　　　3.如果都存在，函数名字也相同，那么函数表达式就会覆盖函数生命的部分
)
事实上执行的是:
　　　　　　　　　　　　　　var getName = function () { alert (4);};

###　第三题　Foo().getName();   //(声明：这题答案应该是４，为了混乱你的思维，就先暂时理解为１吧)
(1).这一题涉及全局变量
    1.首先认清楚什么是全局变量和局部变量的区别
　　　　　　区别：
　　　　　　　　　　　　　　　1.全局变量是在函数范围外声明或在function范围内不加var声明
　　　　　　　　　而局部变量是在函数内使用var声明的变量
　　　　　　　　　例如：
　　　　　　　　　　　　　　　　　var name = "我的名字";
　　　　　　　　　　　　　　　　　function(){
                     myname="你的名字";
　　　　　　　　　　　　　　　　　　　　　var hername="她的名字";
                 }
　　　　　　　　　　　　　　　2.上面name,myname是全局变量,hername是局部变量
　　　　2.全局变量可以使用window对象直接访问到
事实上执行的是:
　　　　　　　　　　　　　　function Foo() {
    　　　　　　　　　　　　　　getName = function () { alert (1); };
    　　　　　　　　　　　　　　return this;
　　　　　　　　　　　　　　}
解释：getName声明为全局是全局函数表达式,可以通过window对象直接调用,执行玩FOO函数返回this指针(即windos对象),然后根据windos对象调用getName全局函数

### 第四题　getName();  
(1).这题其实是调用
　　　　　this．getName();
(2).答案给的是一，其实基本功扎实的应该会知道答案是４,(尼玛，第三题说成是１我就忍了，不过也许是作者只是想传达函数调用的思想)

### 第五题　new Foo.getName(); 
(1).这题答案没有争议
　　实际上与以下类似
　　　　　　function A(){}
      new A();
(2).这里只是多了个FOO,我说了，FOO是对象,这里的getName是FOO的静态属性

### 第六题
(1).这题答案也是对的，涉及函数原型的知识点
事实上执行的是:
```js
(new  FOO()).getName();
```

(2).可能有宝宝会疑问，为什么不是１呢问的好
如果写成

```js
var $this = FOO();
$this.getName();   
```
//这个时候答案是１(如果下面没有其他的getName定义，否则答案还是4)
其实这题调用的是FOO()原型的getName()方法
//本来想多写点，但涉及到prototype原型的方法，喜欢的话以后继续更新

### 第七题
(1).实际上执行

```js
(new　(new FOO()).getName())
```

(2).这一题实际上调用了两次构造函数，与上一题类似，但本体会生成一个新的对象

//不多说，项目中也用不到

## 最后留一道题

```js
function C(){};
C.prototype.test=function(){console.log("mytest");};
C.prototype.test.prototype.testName=function(){console.log("mygod");};
(new (new C()).test()).testName();
```

### 想知道答案的宝宝记得在github上call我
