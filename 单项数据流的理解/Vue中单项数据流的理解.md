补充一下之前的知识：
**params：**
先看一个栗子：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>lesson 16</title>
  <script src="https://unpkg.com/vue@next"></script>
</head>
<body>
  <div id="root"></div>
</body>
<script>
  const app = Vue.createApp({
    data() {
      return { 
            num: 1234,
            a:123,
            b:456,
            c:789
        }
    },
    template: `
      <div>
        <counter :count="num" :a="a" :b="b" :c="c" />
      </div>
    `
  });

  app.component('counter', {
    props: ['count','a','b','c'],
    template: `<div>{{count}}---{{a}}---{{b}}---{{c}}</div>`
  });

  const vm = app.mount('#root');
</script>
</html>

```
这里传了count a b c四个数据，传输没什么大碍，但要考虑一个问题：如果要传输大量数据怎么办？
这是可以用params简化这一过程：

```javascript
  const app = Vue.createApp({
    data() {
      return { 
            params:{
                count: 1234,
                a:123,
                b:456,
                c:789
            }
        }
    },
    template: `
      <div>
        <counter v-bind="params" />
      </div>
    `
  });

  app.component('counter', {
    props: ['count','a','b','c'],
    template: `<div>{{count}}---{{a}}---{{b}}---{{c}}</div>`
  });

  const vm = app.mount('#root');
```
这样写的效果与上面第一个栗子的效果是一样的，但是这里有个小细节不知道大家有没有注意到，就是上面`data数据中的num我写成了count`，对这就是细节，如果这里名字不换成`props里接收数据对应的名字`数据是传输不过来的，所以这个细节要注意。
还有一点就是上面代码中`v-bind="params"`与`:count="params.content" :a="params.a" :b="params.b" :c="params.c"`是等价的，不同的是`v-bind="params"`是一个总写而已。
所以如果要传多个参数的时候，params就是一个不错的选择！

**大小写：**
在讲解之前先做一个小插曲：就是HTML不支持驼峰法的写法，所以在Vue组件化开发的时候一般用 - 作为间隔符，如：

```javascript
  const app = Vue.createApp({
    data() {
      return { num: 1234 }
    },
    template: `
      <div>
        <test :count-abc="num" />
      </div>
    `
  });

  app.component('test', {
    props: ['count-abc'],
    template: `<div>{{count-abc}}</div>`
  });

  const vm = app.mount('#root');
```
中的`:count-abc="num"`，但是上面案例是错误的，打印出来的是NAN，因此该怎么写呢？

```javascript
  const app = Vue.createApp({
    data() {
      return { num: 1234 }
    },
    template: `
      <div>
        <test :count-abc="num" />
      </div>
    `
  });

  app.component('test', {
    props: ['countAbc'],
    template: `<div>{{countAbc}}</div>`
  });

  const vm = app.mount('#root');
```
这样的写法是正确的写法，于是我们发现一个有趣的事情：如果我们用 `-` 连接的方式作为属性名给子组件传输值的时候，在接受的时候要把 `- 写法改写为驼峰式的写法`，这是一个比较特殊的写法，大家把它记住就行。

**单项数据流：**
先看一个栗子：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            num: 1
        }
    },
    template: `
      <div>
        <counter :count="num" />
      </div>
    `
  });

  app.component('counter', {
    props: ['count'],
    template: `<div @click="count += 1">{{count}}</div>`
  });

  const vm = app.mount('#root');
```
上面的写法的想法是通过点击父组件传过来的值count来使每次点击后count的值加一，乍一看感觉没什么问题，但是仔细看看就发现代码写的是由问题的，这里就牵扯到了`单向数据流`的概念了，单项数据流是父组件的数据给到子组件时该数据是只读的，只能使用该数据，但是不能修改父组件的数据的，经过这么一解释相信大家就明白了，这个案例中子组件想改写父组件的数据，这违背了单向数据流的意思，所以程序不会执行且会报错。![上面案例的报错提示](https://img-blog.csdnimg.cn/20210603204303693.png#pic_center)
这是大家可能又有疑问了：那该怎么办去实现上面的效果呢？见下面代码：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            num: 1
        }
    },
    template: `
      <div>
        <counter :count="num" />
      </div>
    `
  });

  app.component('counter', {
    props: ['count'],
    data(){
        return {
            myCount:this.count
        }
    },
    template: `<div @click="myCount += 1">{{myCount}}</div>`
  });

  const vm = app.mount('#root');
```
上面的案例大家一眼就能看懂，就是给子组件赋值父组件的数据值，这样点击修改的就是子组件的数据值了。

**为什么是单项数据流——追根溯源：**
大家看这个栗子：
```javascript
  const app = Vue.createApp({
    data() {
      return { num: 1 }
    },
    template: `
      <div>
        <counter :count="num" />
        <counter :count="num" />
        <counter :count="num" />
      </div>
    `
  });

  app.component('counter', {
    props: ['count'],
    template: `<div @click="count += 1">{{count}}</div>`
  });

  const vm = app.mount('#root');
```
在上面的栗子中，如果子组件可以修改父组件的数据值，那么无论点击哪一个数字，上面打印出来的三个数字都会同时发生改变，（因为它们三个都是引用父组件的数据值的，父组件的数据值发生改变了当然它们都会发生改变了）所以说这样数据安全性更高，也减少了日后开发中一些难找的bug出现。

总结：

```javascript
  // v-bind="params"
  // :content="params.content" :a="params.a" :b="params.b" :c="params.c"
  // 属性传的时候，使用 content-abc 这种命名，接的时候，使用 contentAbc 命名
  // 单项数据流的概念: 子组件可以使用父组件传递过来的数据，但是绝对不能修改传递过来的数据
```

