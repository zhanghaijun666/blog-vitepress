<script setup lang="ts">
import { useData, withBase } from 'vitepress';
import { computed } from 'vue';

const { home } = useBlogConfig();
const { frontmatter, site } = useData();
const author = computed(() => frontmatter.value.author ?? frontmatter.value?.blog?.author ?? home?.author ?? site.value.themeConfig?.blog?.author);
const logo = computed(() => frontmatter.value.logo ?? frontmatter.value?.blog?.logo ?? home?.logo ?? site.value.themeConfig.logo);
</script>

<template>
  <div class="blog-author">
    <img v-if="logo" :src="withBase(logo)" alt="avatar" />
    <p v-if="author">
      {{ author }}
    </p>
  </div>
</template>

<style scoped lang="scss">
.blog-author {
  margin-bottom: 20px;

  img {
    display: block;
    margin: 10px auto;
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background-color: rgba(var(--bg-gradient-home));
  }

  img:hover {
    transform: rotate(666turn);
    transition-duration: 59s;
    transition-timing-function: cubic-bezier(0.34, 0, 0.84, 1);
  }

  p {
    text-align: center;
  }
}
</style>
