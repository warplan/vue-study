主要摘录的是Vue教程中的疑难点，结合demo来加深概念的理解（**持续更新！**）

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

这两个API都是vue2.4.0新增的，教程解释的不是很清楚([demo02](https://github.com/warplan/vue-study/blob/master/demo02/demo02.html))
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

# mixins

mixins接受一个混入对象的数组，实现一个类似浅拷贝的功能。利用mixins可以对组件代码进行抽离及封装。(注：如果传入的是钩子函数，则按照数组的顺序依次执行钩子函数，且会在组件之前执行,跟浅拷贝的顺序有出入)
```javascript
var mixin01 = {
  created() {
    console.log('mixin01')
  },
  data() {
    return {
      name: 'mixin01'
    }
  },
    methods: {
        foo: function() {
            console.log('foo1')
        },
        conflicting: function() {
            console.log('from mixin1')
        }
    }
}

var mixin02 = {
  created() {
    console.log('mixin02')
  },
  data() {
    return {
      name: 'mixin02' 
    }
  },
    methods: {
        foo: function() {
            console.log('foo2')
        },
        conflicting: function() {
            console.log('from mixin2')
        }
    }
}

var vm = new Vue({
    mixins: [mixin01, mixin02],
    created() {
      console.log('vm')
    },
    methods: {
        bar: function() {
            console.log('bar')
        },
        conflicting: function() {
            console.log('from self')
        }
    }
})

// 页面执行时，依次会打印'mixin01','mixin02','vm'
vm.name // 'mixin02'
vm.foo() // 'foo2'
vm.bar() // 'bar'
```

# transition

Vue已经封装好了transition的组件，通过在transition组件上添加name，Vue会根据动画的过程自动添加扩展的class
![image](https://cn.vuejs.org/images/transition.png)


