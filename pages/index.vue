<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

const isMobile = ref(false);
const arrowUp = ref(null);
const isProfile = ref(false);

const transitionName = ref<string>('fade');

const { $gsap: gsap } = useNuxtApp();
const swipeContainer = ref(null);
const startY = ref(0);
const endY = ref(0);
const swipeDirection = ref<any>(null);

const handleTouchStart = (event: any) => {
      startY.value = event.touches[0].clientY;
};

const handleTouchMove = (event: any) => {
      endY.value = event.touches[0].clientY;
};

const handleTouchEnd = () => {
    const deltaY = endY.value - startY.value;

    if (deltaY > 240) {
        swipeDirection.value = 'down';
    } else if (deltaY < -240) {
        swipeDirection.value = 'up';
    }

    console.log(deltaY);

    if(swipeDirection.value == 'up'){
        isProfile.value = true;
    } else if (swipeDirection.value == 'down') {
        isProfile.value = false;
    }
};

const bottom = ref();
const background = ref();
let ctx;

const beforeEnter = (el) => {
  el.style.opacity = 0;
};

const enter = (el, done) => {
  gsap
    .fromTo(
        el, 
        { opacity: 0, y:320, onComplete: done },
        { opacity: 1, duration: 0.2, y:0, onComplete: done }
    );
};

const leave = (el, done) => {
  gsap.to(el, { opacity: 0, y:320, duration: 0.2, onComplete: done });
};

// ウィンドウのサイズが変更されたときにcheckWindowSizeを呼び出す
const checkWindowSize = () => {
  // ウィンドウの幅を取得
  const windowWidth = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth;
  const smartphoneSize = 560;

  // 幅が900px以下の場合の処理
  isMobile.value = windowWidth >= smartphoneSize;
};

const bombUp = () => {
    console.log('ok')
    isProfile.value = true;
}

onMounted(() => {
    // ページ読み込み時にも初回のチェックを行う
    checkWindowSize();

    // ウィンドウのサイズが変更されたときにcheckWindowSizeを呼び出す
    window.addEventListener('resize', checkWindowSize);

    console.log('page read ok')
    ctx = gsap.context((self) => {
        const boxes = self.selector('.arrowUp');
        let tl = gsap
            .timeline()
            .to(".arrowUp", { y: -90, opacity: 0 }).repeat(-1);
    }, bottom.value); // <- Scope!
});

onUnmounted(() => {
  // コンポーネントがアンマウントされたときにリスナーを解除
  window.removeEventListener('resize', checkWindowSize);
  ctx.revert();
});
</script>
<template>
  <div class="background" ref="background" @touchstart="handleTouchStart" @touchmove="handleTouchMove" @touchend="handleTouchEnd">
    <PcBan v-if="isMobile"/>
    <div class="visions">
        <div class="my_vision">My Vision</div>
        <div class="line"></div>
        <div class="head_center">オリジナリティを追求し<br>成功の鍵を生み出す</div>
        <div class="line"></div>
        <div class="company">(仮) 合同会社沖縄新地</div>
    </div>
    <div class="bottom_btn" ref="bottom" @click="bombUp">
        <font-awesome-icon icon="chevron-up" ref="arrowUp" size="2x" class="arrowUp" />
        <p>上にスワイプしてください</p>
    </div>
        <transition
        :name="transitionName"
        @before-enter="beforeEnter"
        @enter="enter"
        @leave="leave"
        >
            <Profile v-if="isProfile"/>
        </transition>
  </div>
</template>