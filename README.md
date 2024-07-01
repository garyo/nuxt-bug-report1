# Nuxt Content Query Bug Report

This example shows a hard-to-debug issue with Nuxt content query and SSR static site generation.

The problem is that the dynamically generated `pages/articles/tags/[tag].vue` page slug, use the `NuxtLayout` default layout. That layout runs a content query in a script:

```ts
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
```

For some reason, that script causes the prerender to fail silently, such that those dynamically rendered tags pages return 404s (or actually the pre-generated query results return 404s):
```
Errors prerendering:
  ├─ /api/_content/query/SzZjDQLhN0.1719800696160.json (3ms)                                          nitro 10:25:00 PM
  │ ├── Error: [404] Document not found!
  │ └── Linked from /articles/tags/blog
  ├─ /api/_content/query/qq4agTT8na.1719800696160.json (2ms)                                          nitro 10:25:00 PM
  │ ├── Error: [404] Document not found!
  │ └── Linked from /articles/tags
  ├─ /api/_content/query/KhUI7un7Nu.1719800696160.json (0ms)                                          nitro 10:25:00 PM
  │ ├── Error: [404] Document not found!
  │ └── Linked from /articles
```

The fix is to have the dyamic pages use a simpler layout that doesn't do this query. But the bug I'm reporting is that the failures during prerender are silent (except for the 404s reported above) so it is very nearly impossible to debug. Took me many hours to even find out it was the layout that was responsible. It seems likely the query is failing but the error is silently discarded; it would be much better if the actual failure were reported when it happens, so the developer can see what query failed with a pointer to their file & line number.
