**数据：**
Vue中数据就是data(){}里面的东西
**方法：**
Vue中方法就是methods
注意：methods里面的this指向的就是vue的实例（仅限于methods里面普通函数的this）

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    message:1
                }
            },
            methods:{
                handle(){
                    console.log(this.message); // 可以通过打印this.message来判断methods里面的this是否指向vue的实例
                }
            },
            template:`
                <div @click="handle">{{message}}</div> 
            `
        });

        const vm = app.mount('#root');
```
注意methods里面箭头函数的this就不是vue的实例了。
methods里面的方法不仅能在绑定事件里使用还能在插值表达式中使用：例如：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world'
                }
            },
            methods:{
                FormateString(string){
                    return string.toUpperCase();
                }
            },
            template:`
                <div @click="handle">{{FormateString(message)}}</div> 
            `
        });

        const vm = app.mount('#root');
```
**计算属性：**
举个例子：
```javascript
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world',
                    count:5,
                    price:2
                }
            },
            computed:{
                total(){
                    return this.count * this.price; 
                }
            },
            template:`
                <div>{{total}}</div> 
            `
        });

        const vm = app.mount('#root');
```
通过methods里面的方法也可以实现：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world',
                    count:5,
                    price:2
                }
            },
            computed:{
                total(){
                    return this.count * this.price; 
                }
            },
            methods:{
                gettotal(){
                    return this.count * this.price;
                }
            },
            template:`
                <div>{{gettotal()}}</div> 
            `
        });

        const vm = app.mount('#root');
```
但两者还是有细微的差别的。

差别：
计算属性：当计算属性依赖的内容发生变更时，才会重新执行计算。
方法：只要页面重新渲染，才会重新计算。
举例：

```javascript
  const app = Vue.createApp({
    data() {
      return {
        message: "hello world",
        count: 2,
        price: 5,
      }
    },
    computed: {
      // 当计算属性依赖的内容发生变更时，才会重新执行计算
      total() {
        return Date.now() + this.count;
        // return this.count * this.price
      }
    },
    methods: {
      formatString(string) {
        return string.toUpperCase();
      },
      // 只要页面重新渲染，才会重新计算
      getTotal() {
        return Date.now();
        // return this.count * this.price;
      },
    },
    template: `
     <div> {{message}} {{total}} </div>
    `
  });
  const vm = app.mount('#root');
```
在上面的案例的控制台中执行 `vm.$data.message = 'bbye';`时message会改变，但页面上的total不会变，原因是total是计算属性，而message不是计算属性依赖的内容，所以不会改变，但如果在控制台中输入 `vm.$data.count = 10;` 的时候页面上的total就会变，原因很简单count是total所依赖的内容，所以只要count一变，页面上total的值就会发生改变。如果将`<div> {{message}} {{total}} </div>`改为`<div> {{message}} {{getTotal}} </div>`，再执行 `vm.$data.message = 'bbye';`时，页面上的getTotal就会发生改变，原因是执行`vm.$data.message = 'bbye';`时页面发生重新渲染，因为getTotal是methods的方法，所以只要页面重新渲染值就会重新计算，所以会发生改变。

如果页面既能采用计算属性又能采用方法的时候建议使用计算属性，因为更高效。

**侦听器：**
watch:{}对象

可以在watch里面做一些异步操作：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world',
                    count:5,
                    price:2
                }
            },
            watch:{
                price(){
                    setTimeout(() => {
                        console.log('price change');
                    },1000)
                }
            },
            computed:{
                total(){
                    return this.count * this.price; 
                }
            },
            methods:{
                gettotal(){
                    return this.count * this.price;
                }
            },
            template:`
                <div>{{gettotal()}}</div> 
            `
        });

        const vm = app.mount('#root');
```
执行`vm.$data.price = 10;`结果是在1秒钟后打印出 `price change`

```javascript
  // data & methods & computed & watch
  // computed 和 method 都能实现的一个功能，建议使用 computed，因为有缓存
  // computed 和 watcher 都能实现的功能，建议使用 computed 因为更加简洁
  const app = Vue.createApp({
    data() {
      return {
        message: "hello world",
        count: 2,
        price: 5,
        newTotal: 10,
      }
    },
    watch: {
      // price 发生变化时，函数会执行
      price(current, prev) {
        this.newTotal = current * this.count;
      }
    },
    computed: {
      // 当计算属性依赖的内容发生变更时，才会重新执行计算
      total() {
        return Date.now() + this.count;
        // return this.count * this.price
      }
    },
    methods: {
      formatString(string) {
        return string.toUpperCase();
      },
      // 只要页面重新渲染，才会重新计算
      getTotal() {
        return Date.now();
        // return this.count * this.price;
      },
    },
    template: `
     <div> {{message}} {{newTotal}} </div>
    `
  });
  const vm = app.mount('#root');
```

