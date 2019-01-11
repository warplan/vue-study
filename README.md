# vue-study
key points and difficulties of vue guide 


### **箭头函数**在vue中使用，应注意this的指向

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
