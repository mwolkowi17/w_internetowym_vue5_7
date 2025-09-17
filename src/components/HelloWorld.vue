<script setup>
import { ref } from 'vue'

defineProps({
  msg: String,
})

const count = ref(0);
const hello = ref("Hi Everyone")
const seen=true;

const items = ["Marcin", "Iwona", "Franek", "Łucja"]

function make(){
  console.log("Hi!")
}
// data() {
//     return {
//       count1: 0
//     }
// }
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <h3 v-if="seen">{{ hello }}</h3>
    <a v-on:click="make">Do something</a>
    <li v-for="item in items">
      {{ item }}
    </li>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Learn more about IDE Support for Vue in the
    <a
      href="https://vuejs.org/guide/scaling-up/tooling.html#ide-support"
      target="_blank"
      >Vue Docs Scaling up Guide</a
    >.
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
