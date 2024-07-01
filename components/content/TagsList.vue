<script setup lang="ts">
const props = defineProps<{
  tags: string|string[]
}>()
</script>

<template>
  <div class="flex flex-wrap text-sm">
    <div v-for="tag in splitTags(tags)"
         :key="tag">
      <Tag :tag="tag"/>
    </div>
  </div>
</template>
