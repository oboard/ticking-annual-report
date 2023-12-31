<template>
  <div class="max-w-lg m-auto">
    <NuxtPage />
  </div>
</template>

<style></style>

<script setup lang="ts">
const appConfig = useAppConfig();

useHead({
  titleTemplate: (titleChunk) => {
    return titleChunk ? `${titleChunk} - ${appConfig.title}` : appConfig.title;
  },
  // 图标
  link: [
    {
      rel: "icon",
      type: "image/svg+xml",
      href: "/favicon.svg",
    },
  ],
});
</script>
