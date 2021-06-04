先看一个通过props接收父组件参数的栗子：

```javascript
  const app = Vue.createApp({
    template: `
      <div>
        <counter msg="hello" />
      </div>
    `
  });

  app.component('counter', {
    props:['msg'],
    template: `
      <div>Counter</div>
    `
  });

  const vm = app.mount('#root');
```
再看一下如果不用props接受时的代码：

```javascript
  const app = Vue.createApp({
    template: `
      <div>
        <counter msg="hello" />
      </div>
    `
  });

  app.component('counter', {
    template: `
      <div>Counter</div>
    `
  });

  const vm = app.mount('#root');
```
比较一下两者的dom结构：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210604150303423.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210604150310796.png#pic_center)
两张图分别是用props接收和未用props接收的效果图，两者对比可见：当父组件传过去的属性值如果子组件不接收时传过来的属性会放在最近的dom标签下.

加`inheritAttrs:false`可以不在子组件中显示这个属性.

```javascript
  const app = Vue.createApp({
    template: `
      <div>
        <counter msg="hello" />
      </div>
    `
  });

  app.component('counter', {
    inheritAttrs:false,
    template: `
      <div>Counter</div>
    `
  });

  const vm = app.mount('#root');
```
`inheritAttrs:false`的意思是不继承父组件的属性.


还有就是当有多个根节点时Non-prpos就不会生效了:

```javascript
  const app = Vue.createApp({
    template: `
      <div>
        <counter msg="hello" />
      </div>
    `
  });

  app.component('counter', {
    // inheritAttrs:false,
    template: `
      <div>Counter</div>
      <div>Counter</div>
      <div>Counter</div>
    `
  });

  const vm = app.mount('#root');
```
这时dom标签下就不会有msg="hello"这个属性了.
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060419405836.png#pic_center)
但是如果我们想让某个div标签下有msg="hello"该怎么办呢?
可以添加`v-bind="$attrs"`使dom标签下有msg="hello":
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210604194329985.png#pic_center)
`v-bind="$attrs"`的意思是把父组件传过来的所有Non-props全部放到那个标签中去.

也可以单一的去获取父组件传过来的Non-props的某一个属性值:

```javascript
  const app = Vue.createApp({
    template: `
      <div>
        <counter msg="hello" msg1="world" />
      </div>
    `
  });

  app.component('counter', {
    // inheritAttrs:false,
    template: `
      <div v-bind:msg="$attrs.msg">Counter</div>
      <div v-bind="$attrs">Counter</div>
      <div>Counter</div>
    `
  });

  const vm = app.mount('#root');
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210604194805800.png#pic_center)

在Vue中Non-props属性不仅可以用在v-bind用在标签上还可以用在生命周期函数里或者JS的表达式里:

```javascript
  const app = Vue.createApp({
    template: `
      <div>
        <counter msg="hello" msg1="hello1" />
      </div>
    `
  });

  app.component('counter', {
    // inheritAttrs: false,
    mounted() {
      console.log(this.$attrs.msg);
    },
    template: `
      <div :msg="$attrs.msg">Counter</div>
      <div v-bind="$attrs">Counter</div>
      <div :msg1="$attrs.msg1">Counter</div>
    `
  });

  const vm = app.mount('#root');
```

