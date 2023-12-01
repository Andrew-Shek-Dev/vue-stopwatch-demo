<template>
  <p v-if="state.seen" @click="add">Now you see me {{ state.nested.count }}</p>
</template>

<script setup lang="ts">
// import { reactive } from 'vue';
// const state:{seen:boolean,nested:{count:number}} = reactive({seen:true, nested:{count:0}})
// function add(){
//   state.nested.count++;
// }
import {ref, type Ref} from "vue";
const state:Ref<{seen:boolean,nested:{count:number}}> = ref({seen:true, nested:{count:0}});
async function add(){
  state.value.nested.count++;
}
</script>

<style scoped>
</style>