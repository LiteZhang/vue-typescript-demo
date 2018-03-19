<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <ul>
      <li>
        <a
          href="https://cn.vuejs.org/"
          target="_blank"
        >
          Vue.js Docs
        </a>
      </li>
      <li>
        <a 
          href="https://www.tslang.cn/index.html"
          target="_blank"
        >
          TypeScript
        </a>
      </li>
    </ul>
   
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'
@Component({
  props: {
    propMessage: String
  }
})
export default class HelloWorld extends Vue {
  // initial data
  msg: string = 'This is a typescript project now'
  // lifecycle hook
  mounted () {
    this.greet()
  }
  // computed
  get computedMsg () {
    return 'computed ' + this.msg
  }
  // method
  greet () {
    // alert('greeting: ' + this.msg)
  }    
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
