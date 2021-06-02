点击按钮数字加一的案例：

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
            count: 0
        }
    },
    methods: {
      handleAddBtnClick() {
         this.count += 1
      }
    },
    template: `
        <div>
            {{count}}
            <button @click="handleAddBtnClick">button</button>
        </div>
    `
  });

  const vm = app.mount('#root');
    </script>
</html>
```
这里是执行了一个函数，也可以不执行函数：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            count: 0
        }
    },
    methods: {
    //   handleAddBtnClick() {
    //      this.count += 1
    //   }
    },
    template: `
        <div>
            {{count}}
            <button @click="count += 1">button</button>
        </div>
    `
  });

  const vm = app.mount('#root');
```
因此可以在里面写一些表达式，但是不建议这样写，因为写在里面代码可读性很差。
**事件对象：**
在Vue中可以通过函数参数得到事件对象：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            count: 0
        }
    },
    methods: {
      handleAddBtnClick(event) {
        console.log(event.target);
        this.count += 1;
      }
    },
    template: `
        <div>
            {{count}}
            <button @click="handleAddBtnClick">button</button>
        </div>
    `
  });

  const vm = app.mount('#root');
```
但如果想通过函数传一些参数，像下面这样：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            count: 0
        }
    },
    methods: {
      handleAddBtnClick(num) {
        this.count += num;
      }
    },
    template: `
        <div>
            {{count}}
            <button @click="handleAddBtnClick(2)">button</button>
        </div>
    `
  });

  const vm = app.mount('#root');
```
那，该怎么写才能获取到事件对象呢？像这样写就完事：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            count: 0
        }
    },
    methods: {
      handleAddBtnClick(num,e) {
        console.log(e.target);
        this.count += num;
      }
    },
    template: `
        <div>
            {{count}}
            <button @click="handleAddBtnClick(2,$event)">button</button>
        </div>
    `
  });

  const vm = app.mount('#root');
```
就是如果想获取原生的事件那就传`$event`，就可以通过这种固定写法获得原生的事件了。

在Vue中如果想要绑定多个函数的时候要记得函数之间用逗号隔开，还有就是每个函数后面都要加括号：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            count: 0
        }
    },
    methods: {
      handleBtnClick() {
        alert(1);
      },
      handleBtnClick1(num,event){
        alert(2);
      }
    },
    template: `
        <div>
            {{count}}
            <button @click="handleBtnClick, handleBtnClick1">button</button>
        </div>
    `
  });

  const vm = app.mount('#root');
```
**事件修饰符：**
1、我们都知道时间是会冒泡的，所以如何组织冒泡？可以通过修饰符`.stop`来阻止冒泡：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            count: 0
        }
    },
    methods: {
      handleDivClick() {
        alert('Div Clicked');
      },
      handleBtnClick() {
        this.count += 1
      },
    },
    template: `
        <div>
            {{count}}
            <div @click="handleDivClick">
                <button @click.stop="handleBtnClick">button</button>
            </div>
        </div>
    `
  });

  const vm = app.mount('#root');
```
**.self：**
该修饰符的意思是只有点击自己的时候才会触发相应事件，当点击相应的子元素的时候不会触发事件：
举例：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            count: 0
        }
    },
    methods: {
      handleDivClick() {
        alert('Div Clicked');
      },
      handleBtnClick() {
        this.count += 1
      },
    },
    template: `
        <div>
            <div @click.self="handleDivClick">
                {{count}}
                <button @click="handleBtnClick">button</button>
            </div>
        </div>
    `
  });

  const vm = app.mount('#root');
```
在这个例子中，点击button时是不会弹出Div Clicked，原因是父元素后面加了.self修饰符，而button是div的子元素，点击button是这个事件不是div自己触发的，是button触发的，所以不会弹出。当点击count时判断count是div自己的标签，所以点击count时会触发。

**.prevent：**
是阻止事件的默认行为，如果将上面例子中.self改为.prevent那么无论点击谁都不会弹出Div Clicked
**.capture:**
这个是阻止捕获。
**.once:**
这个修饰符是事件只执行一次。

**按键修饰符：**
举例：

```javascript
  const app = Vue.createApp({
    methods: {
      handleKeyDown() {
        alert('Key Down');
      },
    },
    template: `
        <div>
            <input @keydown="handleKeyDown" />
        </div>
    `
  });

  const vm = app.mount('#root');
```
如果想通过按下某个键（假设是enter），后执行handleKeyDown这个事件该怎么办？以前是通过原生事件，然后判断原生事件的阿斯科码值是否是enter的阿斯科码值然后判断要不要执行`console.log(111);`，但Vue中的按键修饰符就大大简化了这一过程：

```javascript
  const app = Vue.createApp({
    methods: {
      handleKeyDown() {
        console.log(111);
      },
    },
    template: `
        <div>
            <input @keydown.enter="handleKeyDown" />
        </div>
    `
  });

  const vm = app.mount('#root');
```
像enter这样的修饰符还有 tab delete esc up down left right 这些修饰符
**鼠标修饰符：**

```javascript
  const app = Vue.createApp({
    methods: {
      handleKeyDown() {
        console.log(111);
      },
    },
    template: `
        <div>
            <input @click.right="handleKeyDown" />
        </div>
    `
  });

  const vm = app.mount('#root');
```
这段代码的意思就是当点击鼠标右键的时候就打印出111
鼠标修饰符还有 left middle 
**精确修饰符：**
先看代码：

```javascript
  const app = Vue.createApp({
    methods: {
      handleKeyDown() {
        console.log(111);
      },
    },
    template: `
        <div>
            <input @click.ctrl="handleKeyDown" />
        </div>
    `
  });

  const vm = app.mount('#root');
```
这段代码的意思是当按住键盘ctrl再点击的时候才会触发handleKeyDown这个事件，但是有个问题就是如果按住ctrl再同时按住其他键如：A的时候再点击，也会触发handleKeyDown这个事件，所以该如何解决这个问题？就用到了精确修饰符：exact，见代码：

```javascript
  const app = Vue.createApp({
    methods: {
      handleKeyDown() {
        console.log(111);
      },
    },
    template: `
        <div>
            <div @click.ctrl.exact="handleKeyDown">123</div>
        </div>
    `
  });

  const vm = app.mount('#root');
```
但是Vue里面好像有bug，这个效果不会实现，但Vue文档就是像上面说的那样。

**总结：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>lesson 12</title>
  <script src="https://unpkg.com/vue@next"></script>
</head>
<body>
  <div id="root"></div>
</body>
<script>
  // event, $event
  // 事件修饰符：stop, prevent, capture, self, once, passive
  // 按键修饰符：enter, tab, delete, esc, up, down, left, right
  // 鼠标修饰符：left, right, middle
  // 精确修饰符：exact
  const app = Vue.createApp({
    methods: {
      handleClick() {
        console.log('click')
      },
    },
    template: `
      <div>
        <div @click.ctrl.exact="handleClick">123</div>
      </div>
    `
  });

  const vm = app.mount('#root');
</script>
</html>

```

