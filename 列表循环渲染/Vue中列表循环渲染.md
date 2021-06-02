栗子：

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/vue@next"></script>
</head>
<body>
    <div id="root"></div>
</body>
    <script>
        const app = Vue.createApp({
            data(){
                return {
                    list:['dell','huipu','asus']
                }
            },
            template:`
                <div v-for="item in list">{{item}}</div>
            `
        });


        const vm = app.mount('#root');
    </script>
</html>
```
上面代码中 `<div v-for="item in list">{{item}}</div>` 是核心代码，意思是通过循环，把`list`里面的内容一项一项循环展示到页面上面。

在上面`<div v-for="item in list">{{item}}</div>` 代码中循环可以有两个参数，即`<div v-for="(item,index) in list">{{item}}---{{index}}</div>` ,见index大家应该都明白是什么意思，就是下标，见代码：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    list:['dell','huipu','asus']
                }
            },
            template:`
            <div v-for="(item,index) in list">{{item}}---{{index}}</div>
            `
        });


        const vm = app.mount('#root');
```

数组可以用`v-for` 对象也可以用`v-for`：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    list:['dell','huipu','asus'],
                    firstObject:{
                        firstName:'alex',
                        secondName:'james',
                        job:'teacher'
                    }
                }
            },
            template:`
            <div v-for="(value in firstObject">{{value}}</div>
            `
        });


        const vm = app.mount('#root');
```
对于数组`v-for`就两个参数，第一个参数是值，第二个参数是下标，但对于对象而言，有三个参数，第一个参数是值，第二个是键，第三个是下标。

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    list:['dell','huipu','asus'],
                    firstObject:{
                        firstName:'alex',
                        secondName:'james',
                        job:'teacher'
                    }
                }
            },
            template:`
            <div v-for="(value,key,index) in firstObject">{{value}}---{{key}}---{{index}}</div>
            `
        });


        const vm = app.mount('#root');
```
`v-for`还可以循环数字：
```javascript
        const app = Vue.createApp({
            template:`
            <div v-for="item in 10" >{{item}}</div>
            <button @click="handle">增加</button>
            `
        });


        const vm = app.mount('#root');
```
上面代码意思就是循环1到10之间的数字，将其渲染在页面上。

**:key="index"**
先看代码：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    list:['dell','huipu','asus'],
                    firstObject:{
                        firstName:'alex',
                        secondName:'james',
                        job:'teacher'
                    }
                }
            },
            methods:{
                handle(){
                    this.list.push('hello');
                }
            },
            template:`
            <div v-for="(value,index) in list">{{value}}---{{index}}</div>
            <button @click="handle">增加</button>
            `
        });


        const vm = app.mount('#root');
```
上面代码就是每次点击button的时候页面就会新增一个hello，但每次之前有的就又会重新渲染一遍，这样就会比较浪费性能，但是用`:key="index"`就能提高一些性能，`:key=""`这里面尽量写一些不会重复的东西，比如在循环数组时用`index`，当加上这行代码时，再点击button时渲染页面时Vue底层发现以前的dom结构可以复用，所以就不会再重新渲染之前的dom，这样其实就是性能优化的一种体现。

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    list:['dell','huipu','asus'],
                    firstObject:{
                        firstName:'alex',
                        secondName:'james',
                        job:'teacher'
                    }
                }
            },
            methods:{
                handle(){
                    this.list.push('hello');
                }
            },
            template:`
            <div v-for="(value,index) in list" :key="index">{{value}}---{{index}}</div>
            <button @click="handle">增加</button>
            `
        });


        const vm = app.mount('#root');
```
总之，做循环的时候尽量给一个唯一的`key`值，能起到性能优化的效果。

**后面就是数组数据和对象数据的变更的一些方法，一看就明白，跟原生JS没多大差别：**

```javascript
  const app = Vue.createApp({
    data() {
      return {
        listArray: ['dell', 'lee', 'teacher'],
        listObject: {
          firstName: 'dell',
          lastName: 'lee',
          job: 'teacher'
        }
      }
    },
    methods: {
      handleAddBtnClick() {
        // 1. 使用数组的变更函数 push, pop, shift, unshift, splice, sort, reverse
        // this.listArray.push('hello');
        // this.listArray.pop();
        // this.listArray.shift();
        // this.listArray.unshift('hello');
        // this.listArray.reverse();

        // 2. 直接替换数组
        // this.listArray = ['bye', 'world']
        // this.listArray = ['bye', 'wolrd'].filter(item => item === 'bye');

        // 3. 直接更新数组的内容
        // this.listArray[1] = 'hello'

        // 直接添加对象的内容，也可以自动的展示出来
        // this.listObject.age = 100;
        // this.listObject.sex = 'male';
      }
    },
    template: `
      <div>
        <div v-for="(value, key, index) in listObject" :key="index">{{value}}---{{key}}</div>
        <button @click="handleAddBtnClick">新增</button>
      </div>
    `
  });

  const vm = app.mount('#root');
```
**循环和if的优先级：**
在同一语句中，如果循环和if同时发生时循环的优先级要比if高，且if不会生效：

```javascript
  const app = Vue.createApp({
    data() {
      return {
        listArray: ['dell', 'lee', 'teacher'],
        listObject: {
          firstName: 'dell',
          lastName: 'lee',
          job: 'teacher'
        }
      }
    },
    methods: {
      handleAddBtnClick() {
        // 1. 使用数组的变更函数 push, pop, shift, unshift, splice, sort, reverse
        // this.listArray.push('hello');
        // this.listArray.pop();
        // this.listArray.shift();
        // this.listArray.unshift('hello');
        // this.listArray.reverse();

        // 2. 直接替换数组
        // this.listArray = ['bye', 'world']
        // this.listArray = ['bye', 'wolrd'].filter(item => item === 'bye');

        // 3. 直接更新数组的内容
        // this.listArray[1] = 'hello'

        // 直接添加对象的内容，也可以自动的展示出来
        // this.listObject.age = 100;
        // this.listObject.sex = 'male';
      }
    },
    template: `
      <div>
        <div
          v-for="(value, key, index) in listObject"
          :key="index"
          v-if="key !== 'lastName'"
        >
            {{value}} -- {{key}}
        </div>
        <div v-for="item in 10">{{item}}</div>
        <button @click="handleAddBtnClick">新增</button>
      </div>
    `
  });

  const vm = app.mount('#root');
```
这里的`v-if`语句就不会生效。
但是如果必须要循环和判读的时候，可以像下面这样写（再包一层）：

```javascript
  const app = Vue.createApp({
    data() {
      return {
        listArray: ['dell', 'lee', 'teacher'],
        listObject: {
          firstName: 'dell',
          lastName: 'lee',
          job: 'teacher'
        }
      }
    },
    methods: {
      handleAddBtnClick() {
      }
    },
    template: `
      <div>
        <div
          v-for="(value, key, index) in listObject"
          :key="index"
        >
            <div v-if="key !== 'lastName'">
                {{value}} -- {{key}}
            </div>
        </div>
        <div v-for="item in 10">{{item}}</div>
        <button @click="handleAddBtnClick">新增</button>
      </div>
    `
  });

  const vm = app.mount('#root');
```
再包一层div，将`v-for`和`v-if`分开在不同标签里。
不过上面的写法其实还是有一点小问题的，因为里面多包了一层，所以在展示的时候会多一层div标签，这样跟预期的结构还是有点区别的。（跟1 2 3 外边的结构是有区别的）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602155045371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzUyMjA3NzI4,size_16,color_FFFFFF,t_70#pic_center)
解决上面问题的方法就是使用template标签：

```javascript
  const app = Vue.createApp({
    data() {
      return {
        listArray: ['dell', 'lee', 'teacher'],
        listObject: {
          firstName: 'dell',
          lastName: 'lee',
          job: 'teacher'
        }
      }
    },
    methods: {
      handleAddBtnClick() {
      }
    },
    template: `
      <div>
        <template
          v-for="(value, key, index) in listObject"
          :key="index"
        >
            <div v-if="key !== 'lastName'">
                {{value}} -- {{key}}
            </div>
        </template>
        <div v-for="item in 10">{{item}}</div>
        <button @click="handleAddBtnClick">新增</button>
      </div>
    `
  });

  const vm = app.mount('#root');
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202106021553341.png#pic_center)
template就是一个占位符，它不会在html中展示。
