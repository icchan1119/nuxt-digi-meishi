<script setup lang="ts">
  import { ref, onMounted } from 'vue';
  const { $gsap: gsap } = useNuxtApp();
  const isLoading = ref<boolean>(true);
  const transitionName = ref<string>('fade');
  const beforeEnter = (el) => {
    el.style.opacity = 1;
  };

  const enter = (el, done) => {
    gsap.to(el, { opacity: 1, onComplete: () => { el.style.display = 'block'}});
  };

  const leave = (el, done) => {
    gsap.to(el, { opacity: 0, onComplete: () => { el.style.display = 'none'}});
  };

  onMounted(() => {
    nextTick(() => {
      isLoading.value = false;
    })
  })
</script>
<template>
  <div>
    <transition
        :name="transitionName"
        @before-enter="beforeEnter"
        @enter="enter"
        @leave="leave"
    >
      <Loading v-if="isLoading" />
    </transition>
    <NuxtPage />
  </div>
</template>
