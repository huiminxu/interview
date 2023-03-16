# 引言
## 浏览器支持与回退机制
### 浏览器支持
有时候你可能会发现某个特性已经得到浏览器支持了，但不同浏览器的表现可能还有着细微的差异。比如说，它可能需要一个浏览器前缀，或者在语法上存在细微的差别

这种差异的局面跟浏览器兼容性的局面一样，时刻处在变化之中。因此，在使用某项 CSS 特性之前，不要忘记这方面也是你要做好的功课
```css
{
    background: -moz-linear-gradient(0deg, yellow, red);
    background: -o-linear-gradient(0deg, yellow, red);
    background: -webkit-linear-gradient(0deg, yellow, red);
    background: linear-gradient(90deg, yellow, red);
}
```

### 回退机制
举例来说，当我们像上面那样指定一个渐变色作为背景的时候，应该在前面添加一行实色背景的声明。

```css
{
    background: rgb(255, 128, 0);
    background: -moz-linear-gradient(0deg, yellow, red);
    background: -o-linear-gradient(0deg, yellow, red);
    background: -webkit-linear-gradient(0deg, yellow, red);
    background: linear-gradient(90deg, yellow, red);
}
```

Modernizr（http://modernizr.com/）这样的工具来给根元素（<html>）添加一些辅助类，比如 textshadow 或 notextshadow 等

```css
h1 { color: gray; }
.textshadow h1 {
 color: transparent;
 text-shadow: 0 0 .3em gray;
}
```

如果你想尝试使用的某个 CSS 特性非常新，还可以试试用 @supports
规则来实现回退，可以将其视作浏览器“原生”的 Modernizr。
```css
h1 { color: gray; }
@supports (text-shadow: 0 0 .3em gray) {
 h1 {
 color: transparent;
 text-shadow: 0 0 .3em gray;
 }
```

你不打算使用 Modernizr，也可以自己
写一小段 JavaScript 代码来实现相同的功能——做一些特性检测然后给根元
素加一些辅助类
```js
    var root = document.documentElement; // <html>
    if ('textShadow' in root.style) {
    root.classList.add('textshadow');
    }
    else {
    root.classList.add('no-textshadow');
    }
```
如果我们想要检测某个具体的属性值是否支持，那就需要把它赋给对应
的属性，然后再检查浏览器是不是还保存着这个值。很显然，这个过程会改
变元素的样式，因此我们需要一个隐藏元素
```js
    function testValue(id, value, property) {
    var dummy = document.createElement('p');
    dummy.style[property] = value;
    if (dummy.style[property]) {
    root.classList.add(id);
    return true;
    }
    root.classList.add('no-' + id);
    return false;
    }
```


## Web标准： 是敌还是友  Page33