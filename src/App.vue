<style scoped>

</style>
<template>
  <Suspense>
    <template #default>
      <div>
        <button @click="handleHome">切换到首页</button>
        <button @click="handleCenter">切换到个人中心页面</button>
      </div>
    </template>
    <template #fallback>
      <Loading/>
    </template>
  </Suspense>
  <AsyncHome v-if="isShowHome"/>
  <AsyncCenter v-if="!isShowHome"/>
</template>
<script setup lang="ts">
import {defineAsyncComponent,ref} from "vue";
import Loading from "./components/Loading/index.vue"
import Loading2 from "./components/Loading/index.vue"

const AsyncHome = defineAsyncComponent({
  loader: () => import("./components/Home/index.vue"),
  loadingComponent:Loading2,
  delay:200,
})

const AsyncCenter = defineAsyncComponent({
  loader: async () => {
    await new Promise(resolve => {
      setTimeout(() => {resolve()},2000);
      //加载5秒后返回
    })
    return import("./components/Center/index.vue")
  },
  loadingComponent:Loading2,
  delay:200,
})

const isShowHome = ref<boolean>(true);

const handleHome = () => {
  isShowHome.value = true;
}

const handleCenter = () => {
  isShowHome.value = false;
}
</script>
