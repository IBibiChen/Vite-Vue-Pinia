<template>
  <h1>{{ mainStore.count }}</h1>
  <h1>{{ mainStore.foo }}</h1>
  <h1>{{ mainStore.arr }}</h1>
  <h1>{{ mainStore.bigCount }}</h1>
  <h1>{{ mainStore.bigCount }}</h1>
  <h1>{{ mainStore.bigCount }}</h1>

  <hr>
  <h1>{{ count }}</h1>
  <h1>{{ foo }}</h1>

  <hr>
  <p>
    <button @click="handleChangeState">修改数据</button>
  </p>

</template>

<script setup lang="ts">
import {useMainStore} from "../store";
import {storeToRefs} from "pinia";

const mainStore = useMainStore();

console.log(mainStore.count);

// 这是有问题的，因为这样拿到的数据不是响应式的，是一次性的
// Pinia 其实就是把 state 数据都做了 reactive 处理了
// const {count, foo} = mainStore

// 解决办法，使用 storeToRefs
// 把解构出来的数据做 ref 响应式代理
const {count, foo} = storeToRefs(mainStore)

console.log(count.value)

const handleChangeState = () => {
  // 方式一：最简单的修改数据方式
  // mainStore.count++
  // mainStore.foo += '~'


  // 方式二：如果需要修改多个数据，建议使用 $patch 批量更新
  // mainStore.$patch({
  //   count: mainStore.count + 1,
  //   foo: mainStore.foo += '~',
  //   arr: [...mainStore.arr, 4]
  // })

  // 方式三：更好的批量更新的方式，$patch 一个函数
  // mainStore.$patch(state => {
  //   state.count++;
  //   state.foo += '~';
  //   state.arr.push(4);
  // })


  // 方式四：逻辑比较多的时候可以封装到 actions 中处理
  mainStore.changeState(10);

}


</script>

<style scoped>

</style>
