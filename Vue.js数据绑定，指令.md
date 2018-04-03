# Vue.js数据绑定，指令

---

## 插值

```js
    <span>{{text}}</span>
    //将vue对象中text对应的值渲染至花括号内，一次生效
    <span>{{*text}}</span>
    //将vue对象中text对应的值渲染至花括号内，值发生改变就生效
    <span>{{{text}}}</span>
    //将vue对象中text对应的值渲染至花括号内，其中HTML的标签不会被转换为字符串
```

## 表达式
```js
<span>{{text/100}}</span>
//将text的值渲染后除以100
        
<span>{{var result = text/100}}</span>
//但不可以在花括号里面写语句,报错
```


## 指令

带有v-前缀的特殊特性

### v-if
- 如果v-if的值为false 则 与其对应的元素以及DOM就消失
- 如果v-if的值为true  则 与其对应的元素以及DOM都显现

```js
<div id="app-3">
  <p v-if="seen">现在你看到我了</p>
</div>
<script>
var app3 = new Vue({
  el: '#app-3', 
  data: {
    seen: true
  }
})
</script>
```
### v-show
- 使用方法与v-if相同
- 但是v-if采用的是局部重新编译的方式，渲染量较大，但同时切换速度较快
- v-show仅仅就是为其DOM添加 display:none，只是一个单纯的css属性添加，渲染量小
### v-else
- 必须与v-show或v-if成对出现
- 如果v-if/show的值为false 则与其对应的元素以及DOM就消失，v-else元素就显现
- 如果v-if/show的值为true，与其对应的元素以及DOM都显现，v-else元素就消失
```js
<div id="app-3">
  <p v-if="seen">现在你看到我了</p>
  <p v-else="seen">我消失啦~</p>
</div>
<script>
var app3 = new Vue({
  el: '#app-3', 
  data: {
    seen: true
  }
})
</script>
```
3. v-model
- 用于创建双向数据绑定，通过修改js中对应的对象值可以直接修改表单控件里的值

```js
<input class="ele" type='text' v-model="data.name" placeholder="">
  <script>
  var app3 = new Vue({
    el: '.ele',
    data: {
      data:{
        name:"" //当这里改变的时候 input框的值会对应改变
      }
    }
  })
  </script>
```

- 附加参数
    - number 可以将用户输入的类型自动转换为Number类型,如果转换后为NaN则返回原值
    - lazy 在表单内容发生改变时不立刻改变对应vue对象中的值，而是等到触发change事件时再发生 (有点疑问...）
    - debounce 设置敲击最小延时，用于避免频繁刷新，发送请求