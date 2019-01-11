主要摘录的是vue教程中的疑难点，结合demo来加深概念的理解（**持续更新！**）

# **箭头函数**在vue中使用

不要在选项属性或回调上使用箭头函数([demo01](https://github.com/warplan/vue-study/blob/master/demo01/demo01.html))

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

# inheritAttrs $attrs

这两个API都是vue2.4.0新增的，教程解释的不是很清楚([demo02](https://github.com/warplan/vue-study/blob/master/demo01/demo02.html)
)

inheritAttrs属性默认为true时，子组件的根元素会继承父作用域下（除却props定义）的属性，设置为false，子组件的根元素不会继承父作用域的属性（除class和style外）

$attrs包含的就是父作用域的特性绑定（除了props定义的之外）

```javascript
Vue.component('component-demo', {
    inheritAttrs: true, // 设置true或false
    props: ['label', 'value'],
    template: `
        <div>
            <input 
                :value="$attrs.value" 
                :placeholder="$attrs.placeholder"
                @input="$emit('input', $event.target.value)"
            >
        </div>
    `
})

var vueDemo = new Vue({ el: '#app-demo' })
```
```html
<div id="app-demo">
  <component-demo label="父作用域" value="" name="组件" placeholder="请输入"></component-demo>
</div>
```
渲染结果如下：

```html
<!-- 设置为true时: -->
<div name="组件" placeholder="请输入">
  <input placeholder="请输入">
</div>
<!-- 设置为false时: -->
<div>
  <input placeholder="请输入">
</div>
```

#


