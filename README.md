## Vue官网教程中的疑难点

主要摘录一些vue教程中的一些疑难点，结合加入的一些代码片段加深概念的理解（持续更新！）

#### **箭头函数**在vue中使用，应注意this的指向

不要在选项属性或回调上使用箭头函数

```javascript
var vm1 = new Vue({
    data: {
        a: 1
    },
    created: function() {
        // `this` 指向 vm 实例
        console.log('a is: ' + this.a) // a is: 1
    }
})

var a = '123';
var vm2 = new Vue({
    data: {
        a: 1
    },
    created: () => {
        // `this` 指向 window
        console.log('a is: ' + this.a) // a is: 123
    }
})
```

箭头函数是没有this的，this是根据父级的上下文且是静态生成的

```javascript
// ES6
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}
// ES5
function foo() {
  var _this = this;

  setTimeout(function () {
    console.log('id:', _this.id);
  }, 100);
}
```

#### inheritAttrs $attrs

这两个API都是vue2.4.0新增的，教程解释的不是很清楚，结合例子理解

**inheritAttrs**

属性默认为true

#####


