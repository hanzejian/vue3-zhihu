<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>{{ count }}</h1>
    <h1>{{ double }}</h1>
    <ul>
      <li v-for="number in numbers" :key="number">
        <h1>{{ number }}</h1>
      </li>
    </ul>
    <h1>{{ person.name }}</h1>
    <button @click="increase">+1</button>
  </div>
</template>

<script lang="ts">
import { ref, computed, reactive, toRefs, onMounted, onUpdated, onRenderTracked, watch } from 'vue';
interface DataProps {
  count: number,
  increase: () => void
  double: number,
  numbers: number[],
  person: { name?: string }
}
export default {
  name: 'App',
  // 准备阶段，在所有生命周期函数执行前执行，里面不能引用this
  setup() {
    // const count = ref(0);
    // console.log(count)
    // const double = computed(() => {
    //   return count.value * 2;
    // })
    // const increase = () => {
    //   count.value++
    // }
    // return {
    //   count,
    //   increase,
    //   double
    // }
    onMounted(() => {
      console.log('mounted')
    })
    onUpdated(() => {
      console.log('updated')
    })
    // 起到调试作用，用以观测数据的变化
    onRenderTracked((event) => {
      console.log(event)
    })
    const data: DataProps = reactive({
      count: 0,
      increase: () => { data.count++ },
      double: computed(() => data.count * 2),
      numbers: [0, 1, 2],
      person: {}
    })
    data.numbers[0] = 5;
    data.person.name = 'viking'
    const refData = toRefs(data)
    return {
      ...refData
    }
    // data是一个响应式对象，若是直接取出来则丧失响应性
    // return {
    //   ...data
    // }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
