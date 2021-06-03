组件的理念：将许多复杂的东西拆分为小的东西，然后再组装在一起，从而降低项目维护的成本
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
    // 根组件
    const app  = Vue.createApp({
        template:`<div>
                    <hello />
                    <world />
                </div>`
    });
// 两个子组件
    app.component('hello',{
        template:`<div>hello</div>`
    });
    app.component('world',{
        template:`<div>world</div>`
    });

    const vm = app.mount('#root');
</script>
</html>
```
通过将根组件拆分为两个子组件可以降低代码的开发难度，便于维护。

**组件的复用性：**

```javascript
    // 根组件
    const app  = Vue.createApp({
    // 可以在父组件里面多次使用counter这个组件，它们之间不会相互影响，它们的数据是独立的，不会与其他同名的组件共用
        template:`<div>
                    <counter />
                </div>`
    });
    app.component('counter',{
        data(){
            return {
                count : 1
            }
        },
        template:`<div @click="count += 1">{{count}}</div>`
    });

    const vm = app.mount('#root');
```

全局组件：

```javascript
    // 根组件
    const app  = Vue.createApp({
        template:`<div>
                    <counter-parent />
                    <counter />
                </div>`
    });
    app.component('counter-parent',{
        template:`<counter />`
    });


    app.component('counter',{
        data(){
            return {
                count : 1
            }
        },
        template:`<div @click="count += 1">{{count}}</div>`
    });

    const vm = app.mount('#root');
```
这个案例里面counter这个组件就是全局组件，谁都可以使用。想什么时候用就什么时候用，使用起来简单。

但是全局组件是有弊端的：就这个案例来说，counter这个组件是一直存在的，就算父组件不使用counter这个组件，counter这个组件也是有一直挂载在app上的，占用app空间的，所以这样写对性能有一定的损耗。


局部组件：

```javascript
    const counter = {
        data(){
            return {
                count:1
            }
        },
        template:`<div @click="count += 1">{{count}}</div>`
    }
    // 根组件
    const app  = Vue.createApp({
        components:{ 'dell' : counter },
        template:`<div>
                    <dell />
                </div>`
    });

    const vm = app.mount('#root');
```
在该例子中counter就是一个局部组件，所以在父组件中不能直接使用，需要`components`引进counter后去使用，`还有一点细节就是局部组件counter要写到父组件的上面`。

一般局部组件的`首字母都大写`，为了是区分JS中的驼峰式的写法：

```javascript
    const Counter = {
        data(){
            return {
                count:1
            }
        },
        template:`<div @click="count += 1">{{count}}</div>`
    }
// 局部组件首字母大写
    const HelloWorld = {
        template:`<div>hello world</div>`
    }
    // 根组件
    const app  = Vue.createApp({
        components:{ 
            'dell' : Counter, // 做映射，dell 就是 局部组件的Counter
            'hello-world':HelloWorld
        },
        template:`<div>
                    <dell />
                    <hello-world />
                </div>`
    });

    const vm = app.mount('#root');
```
局部组件使用时要做一个名字与组件间的`映射`，`当不写映射时Vue底层尝试自动进行映射`，如下面代码：

```javascript
    const Counter = {
        data(){
            return {
                count:1
            }
        },
        template:`<div @click="count += 1">{{count}}</div>`
    }

    const HelloWorld = {
        template:`<div>hello world</div>`
    }
    // 根组件
    const app  = Vue.createApp({
        components:{ 
            // 'dell' : Counter,
            // 'hello-world':HelloWorld
            Counter,
            HelloWorld
        },
        template:`<div>
                    <counter />
                    <hello-world />
                </div>`
    });

    const vm = app.mount('#root');
```
在这个案例中`Vue就会自动帮我们进行映射`，但是要注意一点：`就是这个名字不能相差太大`，上面栗子是（counter Counter hello-world HelloWorld），但是我把名字差异改大一点后就不行了：

```javascript
    const Counter = {
        data(){
            return {
                count:1
            }
        },
        template:`<div @click="count += 1">{{count}}</div>`
    }

    const HelloWorld = {
        template:`<div>hello world</div>`
    }
    // 根组件
    const app  = Vue.createApp({
        components:{ 
            // 'dell' : Counter,
            // 'hello-world':HelloWorld
            Counter,
            HelloWorld
        },
        template:`<div>
                    <dell />
                    <hello-world />
                </div>`
    });

    const vm = app.mount('#root');
```
在这个案例中dell与Counter名字差异太大了，Vue就无法自动完成映射，所以会出错，因此还是建议`使用局部组件时在components里面标清除映射关系`。

总结：
  

```javascript
全局组件，只要定义了，处处可以使用，性能不高，但是使用起来简单，名字建议 小写字母单词，中间用横线间隔
局部组件，定义了，要注册之后才能使用，性能比较高，使用起来有些麻烦，建议大些字母开头，驼峰命名
```


