<template>
  <!-- https://vitepress.dev/zh/guide/extending-default-theme#layout-slots -->
  <DefaultTheme.Layout>
    <template #page-top><slot name="page-top" /></template>
    <template #page-bottom><slot name="page-bottom" /></template>

    <template #not-found><slot name="not-found" /></template>
    <template #home-hero-before><slot name="home-hero-before" /></template>
    <template #home-hero-info><slot name="home-hero-info" /></template>
    <template #home-hero-image><slot name="home-hero-image" /></template>
    <template #home-hero-after><slot name="home-hero-after" /></template>
    <template #home-features-before><slot name="home-features-before" /></template>
    <template #home-features-after><slot name="home-features-after" /></template>

    <template #doc-footer-before><slot name="doc-footer-before" /></template>
    <template #doc-before><slot name="doc-before" /></template>
    <template #doc-after><slot name="doc-after" /></template>
    <template #doc-top><slot name="doc-top" /></template>
    <template #doc-bottom><slot name="doc-bottom" /></template>

    <template #aside-top><slot name="aside-top" /></template>
    <template #aside-bottom><slot name="aside-bottom" /></template>
    <template #aside-outline-before><slot name="aside-outline-before" /></template>
    <template #aside-outline-after><slot name="aside-outline-after" /></template>
    <template #aside-ads-before><slot name="aside-ads-before" /></template>
    <template #aside-ads-after><slot name="aside-ads-after" /></template>
  </DefaultTheme.Layout>
</template>
<script setup lang="ts">
import { useData } from 'vitepress';
import DefaultTheme from 'vitepress/theme';
import { nextTick, provide } from 'vue';

const { isDark } = useData();

const enableTransitions = () => 'startViewTransition' in document && window.matchMedia('(prefers-reduced-motion: no-preference)').matches;

provide('toggle-appearance', async ({ clientX: x, clientY: y }: MouseEvent) => {
  if (!enableTransitions()) {
    isDark.value = !isDark.value;
    return;
  }

  const clipPath = [`circle(0px at ${x}px ${y}px)`, `circle(${Math.hypot(Math.max(x, innerWidth - x), Math.max(y, innerHeight - y))}px at ${x}px ${y}px)`];

  await document.startViewTransition(async () => {
    isDark.value = !isDark.value;
    await nextTick();
  }).ready;

  document.documentElement.animate(
    { clipPath: isDark.value ? clipPath.reverse() : clipPath },
    {
      duration: 300,
      easing: 'ease-in',
      pseudoElement: `::view-transition-${isDark.value ? 'old' : 'new'}(root)`,
    }
  );
});
</script>
<style>
::view-transition-old(root),
::view-transition-new(root) {
  animation: none;
  mix-blend-mode: normal;
}

::view-transition-old(root),
.dark::view-transition-new(root) {
  z-index: 1;
}

::view-transition-new(root),
.dark::view-transition-old(root) {
  z-index: 9999;
}

.VPSwitchAppearance {
  width: 22px !important;
}

.VPSwitchAppearance .check {
  transform: none !important;
}
:deep(.VPDoc .content-container) {
  max-width: unset;
}
/* 基本滚动条样式 */
/* 滚动条宽度 */
.VPSidebar::-webkit-scrollbar,
body::-webkit-scrollbar {
  width: 6px;
}
/* 滚动条轨道 */
::-webkit-scrollbar-track {
  background: #f1f1f1;
}
/* 滚动条滑块 */
::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 6px;
}
/* 鼠标悬停时滚动条滑块样式 */
::-webkit-scrollbar-thumb:hover {
  background: #555;
}
</style>
