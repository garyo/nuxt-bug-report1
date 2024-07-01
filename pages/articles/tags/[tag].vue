<script setup lang="ts">
</script>

<template>
  <NuxtLayout> <!-- This shows the bug: a dynamic page can't use the default layout query -->
    <a href="/">Back</a>
    <h1>Articles tagged with {{ tag }}</h1>
  </NuxtLayout>
</template>
[Home](/)
