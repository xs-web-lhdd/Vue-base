```javascript
  // createApp 表示创建一个 Vue 应用, 存储到 app 变量中
  // 传入的参数表示，这个应用最外层的组件，应该如何展示
  // MVVM 设计模式，M -> Model 数据， V -> View 视图， VM -> ViewModel 视图数据连接层
  const app = Vue.createApp({
    data() {
      return {
        message: 'hello world'
      }
    },
    template: "<div>{{message}}</div>"
  });
  // vm 代表的就是 Vue 应用的根组件
  const vm = app.mount('#root');
```
可以通过 `vm.$data`获取到数据，例如：`vm.$data.message = '你好';`，这时页面就会从 `hello world` 转变成 `你好`
具体见图片：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601135644804.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601135656870.png#pic_center)

