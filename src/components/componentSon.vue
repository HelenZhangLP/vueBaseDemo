<template>
  <div>
    <h1>{{msg}}</h1>
    <p>儿子收到 - {{fmsg}}</p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App, from son component',
      lists: ['吃饭','睡觉','打豆豆']
    }
  },
  props: ['fmsg'], //这里一定要注册，只有注册后才能取到
  mounted() {
    this.$emit('fromSon',this.lists)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
  color: #42b983;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
