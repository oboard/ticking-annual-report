<template>
    <div class="text-center mt-8">
      <h1 class="text-2xl font-bold mb-4">前往年度报告</h1>
      <div class="flex items-center justify-center">
        <input v-model="userId" class="border p-2 rounded-md mr-2" type="text" placeholder="填写Ticking ID" />
        <nuxt-link
          :to="{ path: '/', query: { userId: userId } }"
          class="bg-blue-500 text-white px-4 py-2 rounded-md cursor-pointer"
        >
          跳转
        </nuxt-link>
      </div>
      <br/>
      <p class="text-justify px-8">不知道在哪里获取Ticking ID？请在Ticking点击“我”，顶部昵称下方就是Ticking ID</p>
    </div>
  </template>
  
  

<script lang="ts" setup>
import { ref } from "vue";

const userId = ref("");
</script>
