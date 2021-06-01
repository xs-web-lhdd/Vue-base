一个案例说清除条件渲染：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>lesson 10</title>
  <script src="https://unpkg.com/vue@next"></script>
</head>
<body>
  <div id="root"></div>
</body>
<script>
  const app = Vue.createApp({
    data() {
      return {
        show: false,
        conditionOne: false,
        conditionTwo: true
      }
    },
    template: `
      <div v-if="show">Hello World</div>

      <div v-if="conditionOne">if</div>
      <div v-else-if="conditionTwo">elseif</div>
      <div v-else>else</div>

      <div v-show="show">Bye World</div>
    `
  });

  const vm = app.mount('#root');
</script>
</html>

```
上面案例中当使用 `v-if`时里面是`show`，而`show`的值是`false`，所以里面的值不会展示，而这种不会展示是没有html标签的那种不会展示，见图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601174631241.png#pic_center)
同样指令`v-if`可以配合着`v-else-if`和`v-else`使用，它们同样是根据绑定的值是`true`还是`false`来进行判断是否要进行展示，如果不展示就是html标签都没有那种不展示（移除dom元素那种）。它们的执行符合if语句的执行（有过计算机基础的对这应该都不会有有疑问）

还有一个是`v-show`，上面的案例中`v-show="show"`中`show`是`false`，所以也不会进行展示，但这种不展示是有html标签，不过里面的`display`是`none`那种不展示，与`v-if`的不展示是有区别的。见图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210601175317781.png#pic_center)


