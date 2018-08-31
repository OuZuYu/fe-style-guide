
# [vue规范:](https://github.com/pablohpsilva/vuejs-component-style-guide/blob/master/README-CN.md)


# 代码规范
>以下规范总结于《编写可维护的JavaScript》《高性能JavaScript》《JavaScript语言精粹》
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

.className {
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

- 空行 --代码应该由一段一段同系列代码片段组成，而不是一整篇连续文本。

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
构造函数： 大驼峰（开头大写）

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



# 编程实践
## 松耦合：
- 不要在css中使用js， 除非是css自带的 如calc()。


- 不要在js中使用css，除非是给元素定位（style.top, style.left……）


- 不要在js中使用html，如果要判断再渲染，应在vue模版中用v-if v-else-if v-else 判断，而不是在js中返回html字符串。


- 不要在html中使用js（但是个人认为在vue中不必严格遵循这条规则，因为vue是在html中使用@click @keyup 之类的，但是还是尽量指向methods中的方法吧。）

## 抽离应用逻辑
