# Vue.js数据绑定，指令

---

## 插值

```js
    <span>{{text}}</span>
    //将vue对象中text对应的值渲染至花括号内，一次生效
    <span>{{*text}}</span>
    //将vue对象中text对应的值渲染至花括号内，值发生改变就生效
<body>
  <p>Using v-html directive: <span id="demo" v-html="rawHtml"></span></p>
</body>
<script>
  var rawHtml = new Vue({
    el:"#demo",
    data:{
      rawHtml:"<p style='color:red'>f</p>",
    }
  })
</script>
//使用v-html插入html标签
```

## 表达式
```js
<span>{{text/100}}</span>
//将text的值渲染后除以100
        
<span>{{var result = text/100}}</span>
//但不可以在花括号里面写语句,报错

这个称之为Mustache语法 ，即{{}}
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
### v-model
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
    - debounce 设置敲击最小延时，用于避免频繁刷新，发送请求,语法为`debounce = "time"`
    

### v-for
用于重复渲染元素

```
<ul id="demo">
    <li v-for = "item in items" class="item-{{$index}}">
    {{$index}} - {{parentMessage}}{{item.msg}}
    </li>
</ul> 
<!--这个写法是Vue1.0的写法，已经废弃-->
<ul id="demo">
    <li v-for="(item,index) in items" class="item-{{index}}">
    {{index}} - {{parentMessage}}  {{item.msg}}
    </li>
 </ul>
 <!--在v-for后面的括号里跟入两个参数，前一个是item后一个是索引-->
<script>
    var demo = new Vue(
    {
        el:'#demo',
        data:{
            parentMessage:'滴滴',
            items:[
                {msg:'顺风车'},
                {msg:'专车'}
            ]
        }
    }
    )
</script>
```
### v-once
使用v-once指令的元素只在第一次渲染有效不接受数据改变
```
<span v-once>这个将不会改变: {{ msg }}</span>
```
但是其子属性会发生改变

### v-html
这个 `span` 的内容将会被替换成为`rawHtml`的属性值，直接作为 HTML会忽略解析属性值中的数据绑定。
```
<body>
  <p>Using v-html directive: <span id="demo" v-html="rawHtml"></span></p>
</body>
<script>
  var rawHtml = new Vue({
    el:"#demo",
    data:{
      rawHtml:"<p style='color:red'>f</p>",
    }
  })
</script>
//使用v-html插入html标签
```

### v-text

即Mustahce语法 {{}} 修改content.text的值

### v-bind

用于更新HTML特性（attribute）
```
<div v-bind:id="dynamicId"></div>
<img v-bind:src="./img.png">

```

### v-on
```
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
```