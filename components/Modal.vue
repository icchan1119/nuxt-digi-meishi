<script setup lang="ts">
import {ref} from 'vue';
const { $gsap: gsap } = useNuxtApp();
interface Props{
    showModal: boolean;
}

const Props = withDefaults(defineProps<Props>(), {
  showModal: false,
});

const beforeEnter = (el) => {
  el.style.opacity = 0;
};

const enter = (el, done) => {
    gsap
        .fromTo(
            el, 
            { opacity: 0, duration: 0.5, onComplete: done },
            { opacity: 1 ,duration: 0.5, onComplete: done }
        );
    
    gsap.fromTo(
        '.modal-content',
        { scale: 0, duration: 0.5, onComplete: done,ease: 'back.out' },
        { scale: 1, duration: 0.5, onComplete: done,ease: 'back.out' },
    );
};

const leave = (el, done) => {
  gsap.to(el, { opacity: 0, duration: 0.5, onComplete: done,ease: 'back.out' });

  gsap.to('.modal-content', {scale: 0, duration: 0.2})
};
</script>

<template>
    <teleport to="body">
        <transition
            :name="transitionName"
            @before-enter="beforeEnter"
            @enter="enter"
            @leave="leave"
        >
            <div v-if="showModal" class="modal">
                <div class="modal-content">
            <!-- モーダルのコンテンツ -->
                    <slot></slot>
                </div>
            </div>
        </transition>
    </teleport>
</template>