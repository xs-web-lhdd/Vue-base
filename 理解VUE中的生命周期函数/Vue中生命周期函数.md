见图片：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601140126780.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzUyMjA3NzI4,size_16,color_FFFFFF,t_70#pic_center)

```javascript
  // 生命周期函数：在某一时刻会自动执行的函数
  const app = Vue.createApp({
    data() {
      return {
        message: 'hello world'
      }
    },
    // 在实例生成之前会自动执行的函数
    beforeCreate() {
      console.log('beforeCreate')
    },
    // 在实例生成之后会自动执行的函数
    created() {
      console.log('created')
    },
    // 在组件内容被渲染到页面之前自动执行的函数
    beforeMount() {
      console.log(document.getElementById('root').innerHTML, 'beforeMount')
    },
    // 在组件内容被渲染到页面之后自动执行的函数
    mounted() {
      console.log(document.getElementById('root').innerHTML, 'mounted')
    },
    // 当数据发生变化时会立即自动执行的函数
    beforeUpdate() {
      console.log(document.getElementById('root').innerHTML, 'beforeUpdate');
    },
    // 当数据发生变化，页面重新渲染后，会自动执行的函数
    updated() {
      console.log(document.getElementById('root').innerHTML, 'updated');
    },
    // 当 Vue 应用失效是，自动执行的函数    可以在控制台通过 app.unmount() 的方法使Vue失效
    beforeUnmount() {
      console.log(document.getElementById('root').innerHTML, 'beforeUnmount');
    },
    // 当 Vue 应用失效时，且 dom 完全销毁之后，自动执行的函数
    unmounted() {
      console.log(document.getElementById('root').innerHTML, 'unmounted');
    },
  });
  const vm = app.mount('#root');
```

这里补充一个细节：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601142635509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzUyMjA3NzI4,size_16,color_FFFFFF,t_70#pic_center)
该细节是一个判断`template`里面是否有东西的一个判断，如果模板里面有东西就执行左边的路径，如果模板里面没有东西就将节点里面的东西作为模板继续进行下面的生命周期函数的执行。
举例：

```html
  <div id="root">
    <div>{{message}}</div>
  </div>
```
这种就是执行右边的路径（没有template）
