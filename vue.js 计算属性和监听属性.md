# vue.js 计算属性和监听属性






## 侦听

非常类似我们采用面向对象编程撰写代码的时候
```
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    getAnswer: _.debounce(
      function () {
        if (this.question.indexOf('?') === -1) {
          this.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        this.answer = 'Thinking...'
        var vm = this
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      // 这是我们为判定用户停止输入等待的毫秒数
      500
    )
  }
})
</script>
```
## 计算

与方法类似，定义在vue对象下 computed的属性下面

```js
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
  //reversedMessage会返回对应的函数值
</div>
<script>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // this 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
    //这本质上是一个get函数，如果你愿意的话可以写成get形式，同时也可以有自己的set函数来做一些别的操作
  }
})
</script>
```
从结果上来看等同于在vm写写一个method方法，里面添加一个reversedMessage函数，效果相同。

区别在于

计算属性当且仅当与其绑定的数值发生改变时才进行重新计算，否则保持目前的值（存在缓存中），即便你调用很多次也无济于事。方法只要调用就进行重新计算。

如果要用到大量的逻辑运算，甚至要进行异步编程，我们更多运用的是侦听