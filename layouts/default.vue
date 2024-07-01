<template>
  <article>
    <NuxtLink v-if="parentPath && route.path != parentPath" :to="parentPath" class="back">
      <span class="text-primary-900 dark:text-primary-100">
        Back <!-- to {{ parentPath }} -->
      </span>
    </NuxtLink>
    <slot />
  </article>
</template>

<script setup lang="ts">
const route = useRoute()
const { data: page } = await useAsyncData(`content-${route.path}-layout`,
                                          () => queryContent().where({ _path: route.path}).findOne())

const parentPath = computed(
  () => {
    const pathArr = route.path.split('/') // e.g. ["", "articles", "article1"]
    pathArr.pop() // remove last component
    if (pathArr.length <= 1)
      return '/'
    return pathArr.join('/')
  }
)
</script>

<style lang="postcss" scoped>
</style>
