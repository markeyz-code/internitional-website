<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
    <ToastContainer />
    <CustomModalContainer />
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
  description: 'The premier ecosystem and digital community designed exclusively for the modern Medical Laboratory Science Intern & Professional Community.',
  ogTitle: 'InternTional Ecosystem - MLS Intern & Professional Community',
  ogDescription: 'The premier ecosystem and digital community designed exclusively for the modern Medical Laboratory Science Intern & Professional Community.',
  ogImage: '/images/lab_hero_scientist_1789856614097.jpg',
  twitterCard: 'summary_large_image',
});
</script>
