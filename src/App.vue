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
    <component-son :fmsg = "telltoson" v-on:fromSon = "getsonschedule"></component-son>
    <ul v-if = "sonSchedule" >
      <li v-for = "item in sonSchedule"> {{item}} </li>
    </ul>
  </div>
</template>

<script>

import store from './store'
import componentSon from '@/components/componentSon'


export default {
  name: 'App',
  components: {
    componentSon
  },
  data () {
    return {
      title: 'this is a to do list',
      deleteline: 'deleteline',
      items: store.fetch(),
      inputValue: '',
      telltoson: "这里我想跟我儿子说：“我是你爹”",
      sonSchedule: []
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
    },
    getsonschedule: function (item) {
      this.sonSchedule = item 
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

.fC {
  color: #f00
}

.bgc {
  background-color: #000;
}
</style>
