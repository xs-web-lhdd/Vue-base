**v-model：**
input框中的双向绑定：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            message:'hello'
        }
    },
    template: `
        <div>
            {{message}}
            <input v-model="message" />
        </div>
    `
  });

  const vm = app.mount('#root');
```
在上面代码中，如果message的信息改变了，input框里面的东西也会发生相应的改变，因为是双向绑定，所以input框里面的东西改变了，message也会发生改变。
还有一点就是如果用v-model这种写法后就不需要些value了（都双向绑定了还写啥子value啊）

textarea中的双向绑定：
```javascript
  const app = Vue.createApp({
    data(){
        return {
            message:'hello'
        }
    },
    template: `
        <div>
            {{message}}
            <textarea v-model="message" />
        </div>
    `
  });

  const vm = app.mount('#root');
```
这里textarea写法跟html中的写法略微不同（html中textarea写法是\<textarea>\</textarea>，Vue中textarea的写法是但标签\<textraea/>）

checkbox中的v-model：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            message:false
        }
    },
    template: `
        <div>
            {{message}}
            <input type="checkbox" v-model="message" />

        </div>
    `
  });

  const vm = app.mount('#root');
```
这个案例就是checkbox，但注意一点就是message要用bollean类型

checkbox的高级的使用：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            message:[]
        }
    },
    template: `
        <div>
            {{message}}
            Jack<input type="checkbox" v-model="message" value="Jack" />
            Alex<input type="checkbox" v-model="message" value="Alex" />
            James<input type="checkbox" v-model="message" value="James" />

        </div>
    `
  });

  const vm = app.mount('#root');
```
效果图：![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602173325567.png#pic_center)
将message换成数组，再勾选的时候就会往数组中添加相应的value值，**注意：input里面要设value值**

radio按钮：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            message:''
        }
    },
    template: `
        <div>
            {{message}}
            Jack<input type="radio" v-model="message" value="Jack" />
            Alex<input type="radio" v-model="message" value="Alex" />
            James<input type="radio" v-model="message" value="James" />

        </div>
    `
  });

  const vm = app.mount('#root');
```
效果图：![在这里插入图片描述](https://img-blog.csdnimg.cn/20210602173753908.png#pic_center)
因为radio是单选的，再用数组存就不太合适，所以初始化message时用字符串就行。

select的双向绑定：
```javascript
  const app = Vue.createApp({
    data(){
        return {
            message:'请选择内容'
        }
    },
    template: `
        <div>
            {{message}}
            <select v-model="message">
                <option disabled>请选择内容</option>
                <option>A</option>
                <option>B</option>
                <option>C</option>    
            </select>
            

        </div>
    `
  });

  const vm = app.mount('#root');
```
这里可以进行一个简单的优化，将option以循环的方式展现出来：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            message:'请选择内容',
            options:[
                {text:'A', value:'A'},
                {text:'B', value:'B'},
                {text:'C', value:'C'}
            ]
        }
    },
    template: `
        <div>
            {{message}}
            <select v-model="message">
                <option disabled>请选择内容</option>
                <option v-for="item in options" :value="item.value">{{item.text}}</option>   
            </select>
            

        </div>
    `
  });

  const vm = app.mount('#root');
```
这样就方便很多（后端返回的数据全都能展示出来，这个案例假设options的数据是后端返给的数据），注意就是`:value="item.value"`要进行绑定，这样传给后端的数据就不会有问题。

checkbox的自定义：

```javascript
  const app = Vue.createApp({
    data(){
        return {
            message:false,
        }
    },
    template: `
        <div>
           {{message}}
           <input type="checkbox" v-model="message" true-value="选中了" false-value="不选了"  />
            

        </div>
    `
  });

  const vm = app.mount('#root');
```
这里自定义就是通过`true-value`和`false-value`来进行自定义，如果选中了就显示选中了，如果没选就显示不选了。


**v-model的修饰符：**
.lazy：
栗子：
```javascript
  const app = Vue.createApp({
    data(){
        return {
            message:'',
        }
    },
    template: `
        <div>
           {{message}}
           <input v-model="message"   />
            

        </div>
    `
  });

  const vm = app.mount('#root');
```
这个案例是只要输入框中数据一发生变化，同步message的数据就会发生变化，即渲染到页面上的东西就会发生变化，但加上lazy的修饰符后就会不一样，只有光标离开input框之后message才会发生变化。

.number：
该修饰符就是将input的内容转化为number类型再存到message中
```javascript
  const app = Vue.createApp({
    data(){
        return {
            message:'',
        }
    },
    template: `
        <div>
           {{message}}
           {{typeof message}}
           <input v-model.number="message" type="number" />
            

        </div>
    `
  });

  const vm = app.mount('#root');
```


.trem：
该修饰符的意思就是去除input输入框中的空格（只去除数据前后的空格，但去除不了输入有效内容之间的空格）再将数据存到message中：

```javascript
  const app = Vue.createApp({
    data() {
      return {
        message: 'hello',
      }
    },
    template: `
      <div>
        {{message}}
        <input v-model.trim="message"  />
      </div>
    `
  });

  const vm = app.mount('#root');
```

