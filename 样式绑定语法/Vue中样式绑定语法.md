**class方式书写样式：**
数据中通过对象的方式存储样式：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/vue@next"></script>
    <style>
        .red{
            color: red;
        }
        .green{
            color: greenyellow;
        }
    </style>
</head>
<body>
    <div id="root"></div>
</body>
    <script>
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world',
                    // 通过对象来控制样式的展示的时候 前面的内容（red green） 表示要展示的类的名字，后面的true和false用来决定要不要展示这个类
                    classObject:{red:true,green:true} // 这里两个都是true
                }
            },
            template:`<div :class="classObject">{{message}}</div>`
        });

        const vm = app.mount('#root');
    </script>
</html>
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601170637514.png#pic_center)
因为`red:true,green:true`所以都会展示

通过数组的方式展示样式：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/vue@next"></script>
    <style>
        .red{
            color: red;
        }
        .green{
            color: greenyellow;
        }
        .brown{
            color: brown;
        }
    </style>
</head>
<body>
    <div id="root"></div>
</body>
    <script>
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world',
                    classObject:{red:true,green:true},
                    classArray:['red','green',{brown:true}]
                }
            },
            template:`<div :class="classArray">{{message}}</div>`
        });

        const vm = app.mount('#root');
    </script>
</html>
```

下面这个案例只展示script中内容，其他部分与上面一致：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world',
                    classObject:{red:true,green:true},
                    classArray:['red','green',{brown:true}]
                }
            },
            template:`<div :class="classArray">
                        {{message}}
                        <demo class="green" /> 
                    </div>`
        });

        app.component('demo',{
            template:`<div>one</div>` // 子组件只有一个的时候样式写在父组件也可以展示出来
        })

        const vm = app.mount('#root');
```
但一旦子组件有两个及以上的时候样式写在父组件上就不行了：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world',
                    classObject:{red:true,green:true},
                    classArray:['red','green',{brown:true}]
                }
            },
            template:`<div :class="classArray">
                        {{message}}
                        <demo class="green" />
                    </div>`
        });

        app.component('demo',{
            template:`<div>one</div>
                    <div>two</div>`
        })

        const vm = app.mount('#root');
```
因此遇到这种情况可以在子组件中一一添加样式如：

```javascript
        app.component('demo',{
            template:`<div class="green">one</div>
                    <div class="green">two</div>`
        })
```
另一种方法就是使用`$attrs`的方法，到后面子组件与父组件的时候会详细讲：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world',
                    classObject:{red:true,green:true},
                    classArray:['red','green',{brown:true}]
                }
            },
            template:`<div :class="classArray">
                        {{message}}
                        <demo class="green" />
                    </div>`
        });

        app.component('demo',{
            template:`<div :class="$attrs.class">one</div>
                    <div :class="$attrs.class">two</div>`
        })

        const vm = app.mount('#root');
```
**vue中行内样式的书写：**
可以像html标签一样直接写到标签里面：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world',
                    classObject:{red:true,green:true},
                    classArray:['red','green',{brown:true}]
                }
            },
            // 像html一样直接写到标签里面：
            template:`<div style="color:orange;">
                        {{message}}
                    </div>`
        });

        app.component('demo',{
            template:`<div :class="$attrs.class">one</div>
                    <div :class="$attrs.class">two</div>`
        })

        const vm = app.mount('#root');
```
也可以写到字符串里面，然后绑定进去：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world',
                    classObject:{red:true,green:true},
                    classArray:['red','green',{brown:true}],
                    styleString: 'color: yellow;background: orange',
                }
            },
            template:`<div :style="styleString">
                        {{message}}
                    </div>`
        });

        app.component('demo',{
            template:`<div :class="$attrs.class">one</div>
                    <div :class="$attrs.class">two</div>`
        })

        const vm = app.mount('#root');
```
也可以使用vue中的扩展——对象的方式引进去绑定：

```javascript
        const app = Vue.createApp({
            data(){
                return {
                    message:'hello world',
                    classObject:{red:true,green:true},
                    classArray:['red','green',{brown:true}],
                    styleString: 'color: yellow;background: orange',
                    styleObject: {
                        color: 'orange',
                        background: 'yellow'
                    }                    
                }
            },
            template:`<div :style="styleObject">
                        {{message}}
                    </div>`
        });

        app.component('demo',{
            template:`<div :class="$attrs.class">one</div>
                    <div :class="$attrs.class">two</div>`
        })

        const vm = app.mount('#root');
```
这种写法可读性更好，推荐使用。
