<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
    <ToastContainer />
  </div>
</template>

<script setup lang="ts">
import { useHead, useSeoMeta } from '#imports';

useHead({
  titleTemplate: (titleChunk) => {
    return titleChunk ? `${titleChunk} | InternTional Ecosystem` : 'InternTional Ecosystem';
  }
});

useSeoMeta({
  description: 'A complete ecosystem designed exclusively for the demands of the modern Medical Laboratory Science career.',
  ogTitle: 'InternTional Ecosystem',
  ogDescription: 'A complete ecosystem designed exclusively for the demands of the modern Medical Laboratory Science career.',
  ogImage: '/images/lab_hero_scientist_1789856614097.jpg',
  twitterCard: 'summary_large_image',
});
</script>
