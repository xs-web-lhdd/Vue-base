**插值表达式：**
插值表达式就是 {{}}
举例：见代码：

```javascript
        const app = Vue.createApp({
            data() {
                return {
                    message:'hello wrold'
                }
            },
            template: '<div>{{message}}</div>'
        });
        const vm = app.mount('#root');
```
上面代码中{{}}就是插值表达式，里面存放的是变量或者JS的表达式，这个案例里面message指向的就是data里面的message


**v-html：**

```javascript
        const app = Vue.createApp({
            data() {
                return {
                // 这里想给hello world加粗
                    message:'<strong>hello wrold</strong>'
                }
            },
            template: '<div>{{message}}</div>'
        });


        const vm = app.mount('#root');
```
上面那种加粗方式是不对的（会对html的标签进行转义，将其展示出来），见效果图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601144535900.png#pic_center)

要是想给展示出来的hello world加粗需要用到 `v-html`这个指令：
修改后的代码：

```javascript
        const app = Vue.createApp({
            data() {
                return {
                    message:'<strong>hello wrold</strong>'
                }
            },
            template: '<div v-html="message"></div>'
        });
        const vm = app.mount('#root');
```
这里`v-html="message"`的意思是将message以html的形式展示出来，见效果图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601144858992.png#pic_center)
**v-bind：**
给message绑定一个title

```javascript
        const app = Vue.createApp({
            data() {
                return {
                    message:'<strong>hello wrold</strong>'
                }
            },
            template: '<div title="message">hello world</div>'
        });
        const vm = app.mount('#root');
```
这时会发现鼠标放上去展示的并不是data里面的message：
这时就要用v-bind这个指令了，`v-bind:title="message"`这个指令的意思是将title这个属性与data中的messag想绑定，message是啥鼠标放上去就展示啥，完整代码：

```javascript
        const app = Vue.createApp({
            data() {
                return {
                    message:'hello wrold'
                }
            },
            template: '<div v-bind:title="message">hello world</div>'
        });
        const vm = app.mount('#root');
```
v-bind用于一个属性想去绑定一个变量的时候
v-bind其他用法：
	与disabled像绑定来控制input是否可以输入
```javascript
        const app = Vue.createApp({
            data() {
                return {
                    disable: true
                }
            },
            template: '<input v-bind:disabled="disable" />'
        });


        const vm = app.mount('#root');
```
`v-bind`简写是`:`
**插值表达式{{}}中写JS的表达式：**

```javascript
        const app = Vue.createApp({
            data() {
                return {
                    message: 'hello world'
                }
            },
            template: '<div>{{Math.max(1,2)}}</div>'
        });
        const vm = app.mount('#root');
```
记住只能写JS的表达式不能写语句，例如（写语句错误写法）：

```javascript
        const app = Vue.createApp({
            data() {
                return {
                    message: 'hello world'
                }
            },
            template: '<div>{{if(true){console.log(111)}}}</div>' // 里面不能写语句
        });


        const vm = app.mount('#root');
```

**v-once：**
见代码：

```javascript
        const app = Vue.createApp({
            data() {
                return {
                    message: 'hello world'
                }
            },
            template: '<div v-once>{{message}}</div>'
        });

        const vm = app.mount('#root');
```

```javascript
<div v-once>{{message}}</div>
```
这里面的意思就是message只能使用一次，如果再通过vm.$data.message的方法改变message的值的时候就不会重新在页面上渲染message的值了，就不会跟着更新dom了。v-once可以降低无用的渲染，提高性能。
**v-if：**

```javascript
        const app = Vue.createApp({
            data() {
                return {
                    message: 'hello world',
                    show:true
                }
            },
            template: '<div v-if="show">{{message}}</div>'
        });
        const vm = app.mount('#root');
```
这里的`v-if="!show"`就是用来控制div标签展不展示，如果show是true就展示，如果是false就不展示。
**v-on：**
事件绑定：

```javascript
        const app = Vue.createApp({
            data() {
                return {
                    message: 1,
                    show:true
                }
            },
            methods:{
                handle(){
                    alert('click');
                }
            },
            template: '<div v-on:click="handle">{{message}}</div>' // 要把函数写到methods里面
        });

        const vm = app.mount('#root');
```

`v-on`简写是`@`，这里`v-on:click="handle"`与`@click="handle"`是等价的
**动态属性：**

```javascript
        const app = Vue.createApp({
            data() {
                return {
                    message: 'hello world',
                    show:true,
                    name:'title' // 动态属性
                }
            },
            template: '<div :[name]="message">{{message}}</div>' // 动态属性用[]
        });


        const vm = app.mount('#root');
```
这种写法就是动态属性。
**修饰符：**

```javascript
  const app = Vue.createApp({
    data() {
      return {
        message: "hello world",
        show: false,
      }
    },
    methods: {
      handleClick(e) {
        e.preventDefault(); // 阻止表单的跳转行为
      }
    },
    template: `
      <form action="https://www.baidu.com" @click="handleClick">
        <button type="submit">提交</button>
      </form>
    `
  });
  const vm = app.mount('#root');
```
通过`.prevent`修饰符进行阻止：

```javascript
  const app = Vue.createApp({
    data() {
      return {
        message: "hello world",
        show: false,
      }
    },
    methods: {
      handleClick() {
        alert('click')
      }
    },
    template: `
      <form action="https://www.baidu.com" @click.prevent="handleClick">
        <button type="submit">提交</button>
      </form>
    `
  });
  const vm = app.mount('#root');
```
通过修饰符可以简化我们常用代码的编写。

