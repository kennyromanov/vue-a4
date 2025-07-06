## vue-a4

**vue-a4** is a small component for Vue 3 that renders content into paged A4 sheets. It wraps [vue-paged](https://github.com/kennyromanov/vue-paged) and automatically scales pages to fit the container.

### Example
```vue
<script setup lang="ts">
import VueA4 from 'vue-a4';
</script>

<template>
  <VueA4 :paper-width="814">
    <div class="page">Your A4 content</div>
  </VueA4>
</template>
```

## Installation

1. Install **dependencies**:
```shell
npm i vue-a4 vue-paged
```
2. Add the **CSS** import in your stylesheet:
```css
@import 'vue-a4/index.css';
```
3. **You\'re all set!**

---
**vue-a4**  
by Kenny Romanov

