
# [vue规范](https://github.com/pablohpsilva/vuejs-component-style-guide/blob/master/README-CN.md)


# 代码规范
>以下规范总结于《编写可维护的JavaScript》《高性能JavaScript》《JavaScript语言精粹》

***

## 第一部分： 规范

### 编程风格

- 空格与缩进：

```
a && b

for (let i = 0, len = x.length; i < len, i++) {
    // do something
}

let a = b;

let obj = {
    a: false,
    b: 1
    c: '2'
};


if () {

} else {

}

doSomething(1, true);

.class-name {
    position: relative;
}
```
- 分号结尾


- if后只有一条语句 可 不加花括号，否则加上。

```
if (true) return;
if (true) doSomething();
if (true) {
    doSomething();
    return;
}
```

- 空行：__代码应该由一段一段同系列代码片段组成，而不是一整篇连续文本。__

```
if (true) {

    for (let i = 0; len = x.length; i < len; i++) {
        let a = x[i];

        if (a === 1) {

        }

        // 注释注释注释
        doSomething();
    }
}
```

- 命名：
变量、函数： 驼峰。

常量：全大写与下划线组合

构造函数： 大驼峰

[可使用缩写](https://blog.csdn.net/liu_yude/article/details/45317307)

- 字符串
html中用双引号 <div class="clearfix"></div>
js中可用单引号或双引号

- 直接声明，而不是使用构造函数；

```
let a = 'abc';
let b = 1;
let c = true;
let d = {};
let e = [];

// 不好
let a = new String('abc');
let b = new Number(1);
………
```

- 难于理解或可能被误认为错误的代码加注释


- switch:
不加break的话注释说明
不加default的话注释说明


- 所有变量声明在开头
```
function fun() {
    let a = 1,
        b = a + 1,
        c = 'string',
        d = false,
        e,
        f;

    // do something
}
```


- 先声明 后使用
```
let i = 1;
console.log(i);
```
```
// 虽然有函数提升 但是还是先声明后使用
function fun() {
    console.log('fun');
}
fun();
```


- 立刻调用函数加括号

```
(function() {
    // do something
}());
```

- 用for of 代替 for in 或者 for in 加上hasOwnProperty判断
```
for (let prop in object) {
    if (object.hasOwnProperty(prop)) {
        // do something
    }
}
```


- 尽量避免使用continue
逻辑：
如果不行就continue 行则执行某些代码
可改为如果行才执行某些代码 即可避免continue


- 不要使用== != 而是用 === !==


- 不要使用with


- 不要使用eval


- 尽量避免全局变量，非要用时使用命名空间。

---

# 编程实践
## 松耦合：
- 不要在css中使用js， 除非是css自带的 如calc()。


- 不要在js中使用css，除非是给元素定位（style.top, style.left……）


- 不要在js中使用html，如果要判断再渲染，应在vue模版中用v-if v-else-if v-else 判断，而不是在js中返回html字符串。


- 不要在html中使用js

## 事件处理函数
- 分离功能性代码(与应用相关而不是与用户相关的代码)
- 不要把event作为参数传递
- 只让事件处理函数接触event对象

```
// 不好 （没有分离功能性代码）
function handleClick() {
    let popup = document.getElementById('popup');
    popup.style.left = event.clientX + 'px';
    popup.style.top = event.clientY + 'px';
    popup.className = 'reveal';
}


// 不好 （分发event了）
function showPopup(event) {
    let popup = document.getElementById('popup');
    popup.style.left = event.clientX + 'px';
    popup.style.top = event.clientY + 'px';
    popup.className = 'reveal';
}
function handleClick(event) {
    showPopup(event);
}

// 好
function showPopup(x, y) {
    let popup = document.getElementById('popup');
    popup.style.left = x + 'px';
    popup.style.top = y + 'px';
    popup.className = 'reveal';
}
function handleClick(event) {
    showPopup(event.clientX, event.clientY);
}

```


- 除非真的要检测某个值是否为null，否则不要与null比较。

- 使用in检测对象是否存在某属性，而不是让其与undefined或null比较。
> 如果要检测某属性是否某对象的实例属性 使用Object.hasOwnProperty

```
let person = {
    name: 'xxx'
}

// 不好 也许person.age 是可转为 false 的值呢
if (person.age) {

}

// 不好（与undefined或null比较） 也许 person.age 就是 null 呢
if (person.age !== null) {

}

// 好
if ('age' in person) {

}
```

- 不是你的对象不要动
> 不覆盖方法
>
> 不修改方法
>
> 不删除方法


- 以下对象不要动
> 原生对象 Object Array……
>
> DOM对象  document
>
> BOM对象  window
>
> 类库对象

- 避免检测浏览器，非要检测，只检测旧版浏览器。
- 不要推断浏览器
- 解决：直接检测某个特性。

```
// 不好 （检测浏览器）
if (navigator.userAgent.indexOf('MSIE 7') > -1) {

}

// 不好 （推断有某方法就是某浏览器，其他浏览器也可能会实现相同的功能。）
let isIE = !!document.all && document.uniqueID;

// 好
if (document.getElementById) {

}

```

- 一个兼容各浏览器的方法，应当先检测标准方法，再检测各浏览器的方法，再提供通用的方法。

```
// 好
function setAnimate() {
    return window.requestAnimationFrame ||
           window.webkitRequestAnimationFrame ||
           window.mozRequestAnimationFrame ||
           function (callback) {
                window.setTimeout(callback, 6000 / 60);
           };
};
```

***

## 第二部分： 性能

- script 在body闭标签前引入。

- 尽可能使用局部变量，如果跨作用域的值被引用一次以上，则缓存。

```
// 不好
function init() {
    let bd = document.body,
        links = document.getElementsByTagName('a');
        i = 0,
        len = links.length;

    while(i < len) {
        update(links[i++]);
    }

    document.getElementById('goBtn').onclick = function() {
        start();
    }

    bd.className = 'active';
}

// 好 document处于作用域链中很深的位置，并且多次引用，应缓存起来。
function init() {
    let doc = document,
        bd = doc.body,
        links = doc.getElementsByTagName('a');
        i = 0,
        len = links.length;

    while(i < len) {
        update(links[i++]);
    }

    doc.getElementById('goBtn').onclick = function() {
        start();
    }

    bd.className = 'active';
}
```

- 不要使用with，with使得局部变量处于作用域链中第二的位置，访问代价更高。
> 其实try catch同理：会把错误对象推倒作用域首位，但是try catch是非常有用的语句，不建议弃用。
> 解决方法是将错误委托给函数。

```
// 好
try {
    func();
} catch(err) {
    handleErr(err); // 委托给一个函数处理
}

```

- 小心使用闭包，闭包使得活动对象无法销毁，存在更多内存开销，可配合以上说的将跨作用域的变量缓存，减轻负担。

- 对象成员嵌套越深，读取越慢，localhost.href 总是比 window.location.href 要快。

- 如果一个对象属性被引用两次以上，就应该缓存起来，避免重复读取。

```
// 不好
function hasEitherClass(ele, className1, className2) {
    return element.classNam === className1 || element.classNam === className2;
}

// 好
function hasEitherClass(ele, className1, className2) {
    let curClassName = element.classNam;
    return curClassName === className1 || curClassName === className2;
}
```

-

- 访问dom非常低效，应减少dom访问。

```
// 不好
function innerHTMLLoop () {
    for (var count = 0; count < 1000; count++) {
        document.getElementById('here').innerHTML += 'a';
    }
}

// 好
function innerHTMLLoop () {
    var content = '';

    for (var count = 0; count < 1000; count++) {
        count += 'a';
    }
    document.getElementById('here').innerHTML = content;
}
```

- 减少重排与重绘，避免重排队列强制刷新。

- 使用事件委托。

- 提高循环性能可减少迭代次数或减轻每次迭代处理的事务

- 避免for in循环，除非要循环对象的原型属性。

- 循环时缓存length，避免多次读取。

- 颠倒循环更快（减少了一次判断）。

```
let arr = [3,2,1];
for(let i = arr.length; i--; ) {
    console.log(arr[i]); // 1 2 3
}

let j = arr.length;
while(j--) {
    console.log(arr[j]); // 1 2 3
}
```

- 如果循环次数在1000以上，使用达夫设备。

- for循环比forEach快，因forEach要对每项调用方法。

- 条件数量大时 switch 比 if-else 快。

- if else优化
> 确保最可能出现的条件放首位
> if else 嵌套
> 用查找表的方法代替

```
// 好 if else 嵌套
if (value < 6){
  if (value < 3){
    if (value == 0){
      return result0;
    } else if (value == 1){
      return result1;
    } else {
      return result2;
    }
  } else {
  if (value == 3){
      return result3;
    } else if (value == 4){
      return result4;
    } else {
      return result5;
    }
  }
} else {
  if (value < 8){
    if (value == 6){
      return result6;
    } else {
      return result7;
    }
  } else {
    if (value == 8){
      return result8;
    } else if (value == 9){
      return result9;
    } else {
      return result10;
    }
  }
}

// 好 查找表代替if else
// 数组
var results = [result0, result1, result2, result3...]
return results[value];

//对象
var obj = {
    num1: '未发货',
    num2: '已发货',
    num3: '已到货',
}
//对象成员查找
return obj[num1];
```

- 若一个任务运行过长（超过100毫秒)，则利用定时器分割成小任务或者使用Web Workers。

```
function save () {
    let tasks = [open, write, close];

    setTimeout(() => {
        let task = tasks.shift();
        task();

        if (tasks.length) {
            setTimeout(arguments.callee, 25);
        }
    }, 25);
}
```

- 避免eval setTimeout setInterval 的双重求值

- 避免每次都做同一件事

```
// 不好 函数每次执行都判断一次是否支持addEventListener
function addHandle(target, eventType, handler) {
    if (target.addEventListener) {
        target.addEventListener(eventType, handler, false);
    } else {
        target.attachEvent('on' + eventType, handler);
    };
}

// 好 第一次执行判断，判断完成覆盖掉原方法。
function addHandle(target, eventType, handler) {
    if (target.addEventListener) {
        addHandle = function() {
            target.addEventListener(eventType, handler, false);
        }
    } else {
        addHandle = function() {
            target.attachEvent('on' + eventType, handler);
        }
    };
}

// 好
let addHandle = document.body.addEventListener ?
                addHandle = function() {
                   target.addEventListener(eventType, handler, false);
                }:
                function() {
                    target.attachEvent('on' + eventType, handler);
                };
```

## 第三部分

待续……