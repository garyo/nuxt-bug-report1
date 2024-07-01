<script setup lang="ts">
const props = defineProps<{
  tag: string
  }>()
</script>

<template>
  <NuxtLink class="font-normal decoration-slate-400
                   text-gray-600 dark:text-gray-400
                   bg-sky-200 dark:bg-sky-700 rounded-full px-1 mx-[2px]"
            :to="`/articles/tags/${props.tag}`"
  >
    {{props.tag}}
  </NuxtLink>
</template>
