# vue-first-pro

> this is my first vue project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader)



双向绑定

强调组件的概念

.vue = + +

vue-cli -- vue 的脚手架

npm install -g vue-cli ## vue 的脚手架工具
vue init webpack my-project ## 使用 webpack 模板，使用 webpack 进行打包
npm install #安装依赖包
npm run dev #运行项目
.vue -> webpack -> .html + .js + .css

vue 选项 watch
new Vue({
    data: {
        p: 'property',
        b: []
    },
    methods: {
        doSomething: function() {
            return this.p + ' change property content'
        }
    },
    watch: { //对 data 中的属性进行监听
        'p' : function (val, oldVal) {
            console.log(val, oldVal)
        }
    }
})
vue 中的模板指令
数据渲染 v-text v-html {{}}
<template>
  <div id="app">
    <p>{{ a }}</p>
    <p v-text = "a"></p>
    <p v-html = "a"></p>
  </div>
</template>
<script>
export default {
  name: 'App',
  data() {
    return {
      a : '<span>test v-text && v-html</span>'
    }
  }
}
</script>
//执行结果
<div id="app">
    <p>&lt;span&gt;test v-text &amp;&amp; v-html&lt;/span&gt;</p>
    <p>&lt;span&gt;test v-text &amp;&amp; v-html&lt;/span&gt;</p>
    <p><span>test v-text &amp;&amp; v-html</span></p>
</div>
由以上执行结果可知，表达式 {{}} 与 v-text 会将 html 标签当作文本解析，而 v-html 会解析标签，{{}} 是 v-test 的简写

控制模块隐藏 v-if、v-show
<template>
  <div id="app">
    <p v-if = "isShow">v-if</p>
    <p v-show = "isShow">v-show</p>
  </div>
</template>
<script>
export default {
  name: 'App',
  data() {
    return {
      isShow: false
    }
  }
}
</script>
//解析结果
<p style="display: none;">v-show</p>
由以上解析结果可知， v-show 在 isShow 为false 的情况下，只是用 css 在做隐藏，dom 可见

v-for
<template>
  <div id="app">
    <ul v-for = "item in items">
      <li>{{item.content}}</li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'App',
  data() {
    return {
      items: [
        {content:'apple'},
        {content:'banana'},
        {content:'pear'},
        {content:'peach'}
      ]
    }
  }
}
</script>
//解析结果
<ul><li>apple</li><li>banana</li><li>pear</li><li>peach</li></ul>
v-on 事件绑定
<template>
  <div id="app">
    <button v-on:click="clickEvent">clickEventA</button>
    <button @click="clickEvent">clickEventB</button> <!-- @click 是 v-on:click 的简写 -->
  </div>
</template>
<script>
export default {
  name: 'App',
  methods: {
    clickEvent: function(e) {
      console.log(e.target)
    }
  }
}
</script>
v-bind 属性绑定
<template>
  <div id="app">
    <p>{{ a }}</p>
    <p v-text = "a"></p>
    <p v-html = "a"></p>
    <p v-if = "isShow">v-if</p>
    <p v-show = "isShow">v-show</p>
    <button v-on:click="clickEvent">clickEventA</button>
    <button @click="clickEvent">clickEventB</button>
    <ul>
      <li v-for = "item in items">{{item.content}}</li>
    </ul>
    <button v-bind:class="fS">fS</button>
    <button v-bind:class="[fS, fC, bgc]">[fS, fC, bgc]</button>
    <button v-bind:class="[fS, {bgc: isShowStyle, fC : true}]">[fS, {bgc: isShowStyle, fC : true}]</button>
    <button v-bind:style = "isShowStyle ? {color: green} : {color: 'red'}">isShowStyle ? {color: green} : {color: 'red'}</button>
  </div>
</template>
<script>
export default {
  name: 'App',
  data() {
    return {
      isShowStyle: false,
      fS: 'fS',
      fC: 'fC',
      bgc: 'bgc'
    }
  },
  methods: {
    clickEvent: function(e) {
      console.log(e.target)
    }
  }
}
</script>
<style>
.fS {
  font-size: 20px;
}
.fC {
  color: #f00
}
.bgc {
  background-color: #000;
}
</style>
v-model 绑定表单，在表单上创建双向绑定
<template>
  <div id="app">
    <input type="text" v-model = "inputValue" @keyup.enter = "pushItem" />
    <ul>
      <li v-for = "item in items" :class = "{ deleteline: item.toggle } ">
        <input type = "checkbox" @click = "toggleDone(item)"/>
        {{item.content}}
      </li>
    </ul>
  </div>
</template>
<script>
import store from './store'
export default {
  name: 'App',
  data () {
    return {
      title: 'this is a to do list',
      deleteline: 'deleteline',
      items: store.fetch(),
      inputValue: ''
    }
  },
  methods: {
    toggleDone: function (item) {
      item.toggle = !item.toggle
    },
    pushItem: function () {
      this.items.push({
        content: this.inputValue,
        toggle: false
      })
      this.inputValue = ''
    }
  },
  // 监听 items 里的数据，数据变化后进行存取
  watch: {
    items: {
      handler: function (val, oldval) {
        store.save(val)
      },
      deep: true
    }
  }
}
</script>
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.deleteline {
  text-decoration: line-through;
}
</style>
组件导入后需要注册
Unknown custom element: <componentSon> - did you register the component correctly? For recursive components, make sure to provide the "name" option.
//以下是正确处理方式
<template>
  <div id="app">
    <h1>{{title}}</h1>
    <input type="text" v-model = "inputValue" @keyup.enter = "pushItem" />
    <ul>
      <li v-for = "item in items" :class = "{ deleteline: item.toggle } ">
        <input type = "checkbox" @click = "toggleDone(item)"/>
        {{item.content}}
      </li>
    </ul>
    <component-son></component-son> <!-- 注意这里组件驼峰命名后的解析方式 -->
  </div>
</template>
<script>
import store from './store'
import componentSon from '@/components/componentSon'
export default {
  name: 'App',
  components: {
    componentSon //注册组件
  },
}
</script>
vuejs 组件之间通信 - props
父组件与子组件通信 - props
子组件与父组件通信 - $.emit.
