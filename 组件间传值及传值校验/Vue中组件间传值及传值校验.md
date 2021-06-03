见案例：

```javascript
  const app = Vue.createApp({
    template: `
      <div><test content="hello world" /></div>
    `
  });

  app.component('test', {
    props:['content'], // 接收父组件传来的值
    template: `<div>{{content}}</div>`
  });

  const vm = app.mount('#root');
```
这个案例中，父组件给子组件传了一些值：content，子组件通过`props`这个属性进行接受，然后子组件就可以使用这个动态的内容。

静态传参：
```javascript
  const app = Vue.createApp({
    template: `
      <div><test content="123" /></div>
    `
  });

  app.component('test', {
    props:['content'],
    template: `<div>{{typeof content}}</div>`
  });

  const vm = app.mount('#root');
```
就是想传一个数字，但传过来的是一个字符串，所以该怎么办？不急，可以通过一个动态传参的方式：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            num: 123
        }
    },
    template: `
      <div><test v-bind:content="num" /></div>
    `
  });

  app.component('test', {
    props:['content'],
    template: `<div>{{typeof content}}</div>`
  });

  const vm = app.mount('#root');
```
通过上面动态传参的方式，就传过去了一个数字。

所谓静态传参就是`没有用v-bind进行绑定可变的内容`，就是写死的东西（往往穿的是字符串），但是动态传参是通过`v-bind传一些可变的内容`，传的内容是由data里面的数据所决定的，因此是动态传参。



**校验：**

**content：type required default**
```javascript
  const app = Vue.createApp({
    data(){
        return {
            num: 123
        }
    },
    template: `
      <div><test :content="num" /></div>
    `
  });

  app.component('test', {
    props:{
        content:String // 要求传的内容的类型
    },
    template: `<div>{{typeof content}}</div>`
  });

  const vm = app.mount('#root');
```
将props类型由数组的形式改为对象的形式，content就是要求传过来的内容的类型，这个案例是要求传String类型，但传过来的是Number类型，因此程序回会报错：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603155941562.png#pic_center)
这里content可以校验：String, Boolean, Array, Object, Function, Symbol这么些类型。

这里要特别说一下传函数的作用：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            num:() => { alert(123); }
        }
    },
    template: `
      <div><test :content="num" /></div>
    `
  });

  app.component('test', {
    props:{
        content:Function
    },
    methods:{
        handleClick(){
            alert(456);
            this.content(); // content是一个函数，所以就可以去执行
        }
    },
    template: `<div @click="handleClick">{{typeof content}}</div>`
  });

  const vm = app.mount('#root');
```
上面的案例就展示了Function的用法。

**required：**

```javascript
  const app = Vue.createApp({
    data() {
      return { num: 1234 }
    },
    template: `
      <div><test :content="num" /></div>
    `
  });

  // type:String, Boolean, Array, Object, Function, Symbol
  // required 必填
  // default 默认值
  app.component('test', {
    props: {
      content: {
        type: Number,
        required:true
      }
    },
    template: `<div>{{content}}</div>`
  });

  const vm = app.mount('#root');
```
上面的required意思是是否要传值，true的意思是必须要传值，false是可以不传值，在上面案例中，required值是true即必须要传值，如果上面案例没有传值程序将会报错。

**default：**

```javascript
  const app = Vue.createApp({
    data() {
      return { num: 1234 }
    },
    template: `
      <div><test :content="num" /></div>
    `
  });

  // type:String, Boolean, Array, Object, Function, Symbol
  // required 必填
  // default 默认值
  app.component('test', {
    props: {
      content: {
        type: Number,
        // required:true
        default:446
      }
    },
    template: `<div>{{content}}</div>`
  });

  const vm = app.mount('#root');
```
default意思就是默认的值，上面案例中如果传值了那么结果就是传的值，若没传值，结果就是default对应的值。default的值类型不仅可以是Number，也可以是一个函数（下面的案例就是函数）。

校验传入的值是否大于1000：
```javascript
  const app = Vue.createApp({
    data() {
      return { num: 1234 }
    },
    template: `
      <div><test :content="num" /></div>
    `
  });

  // type:String, Boolean, Array, Object, Function, Symbol
  // required 必填
  // default 默认值
  app.component('test', {
    props: {
      content: {
        type: Number,
        validator: function(value) {
          return value < 1000;
        },
        default: function() {
          return 456;
        }
      }
    },
    template: `<div>{{content}}</div>`
  });

  const vm = app.mount('#root');
```
这里用`validator`进行深度校验，校验传入的值的大小是否超过1000，不超过1000，`validator`就返回true，若超过1000，程序就会报错（传的值还是会在页面上显示出来）：![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060316210631.png#pic_center)

