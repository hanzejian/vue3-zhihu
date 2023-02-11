<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>{{ count }}</h1>
    <h1>{{ double }}</h1>
    <h1>{{ greetings }}</h1>
    <button @click="openModal">openModal</button><br/>
    <modal :isOpen="modalIsOpen" @close-modal="onModalClose">
      My Modal!!!
    </modal>
    <h1 v-if="loading">Loading!...</h1>
    <img v-if="loaded" :src="result[0].url" alt="">
    <h1>X:{{ x }},Y:{{ y }}</h1>
    <button @click="increase">+1</button>
    <button @click="updateGreeting">Update Title</button>
  </div>
</template>

<script lang="ts">
import { ref, computed, reactive, toRefs, watch, onMounted, onUnmounted } from 'vue';
import useMousePosition from './hooks/useMousePosition'
import useURLLoader from './hooks/useURLLoader'
import Modal from './components/Modal.vue'
interface DataProps {
  count: number,
  increase: () => void
  double: number
}
interface DogResult {
  message: string;
  status: string;
}
interface CatResult {
  id: string;
  url: string;
  width: number;
  height: number;
}
export default {
  name: 'App',
  components: {
    Modal
  },
  // 准备阶段，在所有生命周期函数执行前执行，里面不能引用this
  setup() {
    // 起到调试作用，用以观测数据的变化
    const data: DataProps = reactive({
      count: 0,
      increase: () => { data.count++ },
      double: computed(() => data.count * 2)
    })

    /**
     * 对比起mixin有两大优点
     * 1.这样做可以清楚这两个值的来源
     * 2.可以对x,y设置任何的别名，减少命名冲突的风险
     * 3.可以脱离组件的逻辑
     */
    const { x, y } = useMousePosition();
    // const { result, loading, loaded } = useURLLoader<DogResult>('https://dog.ceo/api/breeds/image/random')
    const { result, loading, loaded } = useURLLoader<CatResult>('https://api.thecatapi.com/v1/images/search?limit=1')
    watch(result, () => {
      if (result.value) {
        // console.log('value',result.value[0].url)
      }
    })
    const greetings = ref('');
    const updateGreeting = () => {
      greetings.value += 'Hello! '
    }

    // watch可以同时监听多个值，若一个数据在响应对象里面，则需要用函数的方法监听
    watch([greetings, () => data.count], (newVal, oldVal) => {
      console.log(newVal);
      console.log(oldVal);
      document.title = 'update' + greetings.value + data.count
    })
    const refData = toRefs(data)
    const modalIsOpen = ref(false)
    const openModal = () => {
      modalIsOpen.value = true
    }
    const onModalClose = () => {
      modalIsOpen.value = false
    }
    return {
      ...refData,
      greetings,
      updateGreeting,
      x,
      y,
      result,
      loading,
      loaded,
      openModal,
      onModalClose,
      modalIsOpen
    }
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
