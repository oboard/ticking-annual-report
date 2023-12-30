<!-- Ticking年度报告H5页面 -->

<template>
  <div class="carousel carousel-vertical h-screen w-full">
    <div class="relative">

        <!-- 背景图片，位于/1.png -->
        <div class="absolute top-0 left-0 right-0 bottom-0">
            <img src="/1.gif" alt="背景图片" class="w-full h-full object-contain">
            
        </div>
        
        <!-- 年度报告标题 -->
        <!-- <div class="absolute top-0 left-0 right-0 flex justify-center items-center h-16 bg-white">
            <div class="text-2xl font-bold text-black">年度报告</div>
        </div> -->
    </div>
    <div class="relative">

        <!-- 背景图片，位于/1.png -->
        <div class="absolute top-0 left-0 right-0 bottom-0">
            <img src="/2.gif" alt="背景图片" class="w-full h-full object-contain">
        </div>
        
        <!-- 年度报告标题 -->
        <!-- <div class="absolute top-0 left-0 right-0 flex justify-center items-center h-16 bg-white">
            <div class="text-2xl font-bold text-black">年度报告</div>
        </div> -->
    </div>
  </div>
</template>

<style>
.carousel > * {
  @apply carousel-item h-screen w-full;
}
</style>
