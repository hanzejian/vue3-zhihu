<template>
<!-- 瞬间移动组件：用teleport组件包裹可以将这个组件去到id值为'modal'的位置（与app同级） -->
<teleport to="#modal">
    <div id="center" v-if="isOpen">
        <h2><slot>this is a modal</slot></h2>
        <button @click="buttonClick">Close</button>
    </div>
</teleport>
    
</template>
<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
    props: {
        isOpen: Boolean
    },
    emits: {
        'close-modal': null
    },
    setup(props, context) {
        const buttonClick = () => {
            context.emit('close-modal')
        }
        return {
            buttonClick
        }
    }
})
</script>
<style>
#center {
    width: 200px;
    height: 200px;
    border: 2px solid black;
    background: white;
    position: fixed;
    left: 50%;
    top: 50%;
    margin-left: -100px;
    margin-top: -100px;
}
</style>
