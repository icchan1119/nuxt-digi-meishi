<script setup lang="ts">
import { Ref } from 'vue';
import { ref, onMounted, onUnmounted } from 'vue';
// import { Particles } from 'nuxt-particles';

const isMobile: Ref<boolean> = ref(false);
const isWelcomeBoard: Ref<boolean> = ref(true);
const { $gsap: gsap } = useNuxtApp();
const modal_message: Ref<string> = ref("");
const modal_memory: Ref<string> = ref("");

const isModalOpen: Ref<boolean> = ref(false);

const openModal = (): void => {
      isModalOpen.value = true;
};

const closeModal = (): void => {
    isModalOpen.value = false;
};

const flyLink = (): void => {
    location.href = modal_memory.value;
}

// ウィンドウのサイズが変更されたときにcheckWindowSizeを呼び出す
const checkWindowSize = (): void => {
  // ウィンドウの幅を取得
  const windowWidth = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth;
  const smartphoneSize = 560;

  // 幅が900px以下の場合の処理
  isMobile.value = windowWidth >= smartphoneSize;
};

onMounted(() => {
    // ページ読み込み時にも初回のチェックを行う

    const mainMovie = document.getElementById('video') as HTMLVideoElement;

    checkWindowSize();

    // ウィンドウのサイズが変更されたときにcheckWindowSizeを呼び出す
    window.addEventListener('resize', checkWindowSize);

    setTimeout(() => {
      isWelcomeBoard.value = false;
    }, 4000);

    const playCount: number = 7.5;

    if (mainMovie) {
        setTimeout(() => {
          mainMovie.play();
          setTimeout((done: () => void) => {
             // gsap.set('.bbs', {opacity: 0});
            gsap.to('.bbs',{ opacity: 1, onComplete: done});
            gsap.to('.bbs2',{ opacity: 1, onComplete: done});
            gsap.to('.bbs3',{ opacity: 1, onComplete: done});
            gsap.to('.bbs4',{ opacity: 1, onComplete: done});
            gsap.to('.bbs5',{ opacity: 1, onComplete: done});
            gsap.to('.bbs6',{ opacity: 1, onComplete: done});
            gsap.to('.bbs7',{ opacity: 1, onComplete: done});
            gsap.to('.bbs8',{ opacity: 1, onComplete: done});

            setTimeout(() => {
              gsap.to('.my_profile_card',{ opacity: 1, onComplete: done});
            }, 500);
          }, playCount*1000);
        }, 3500);
    }
});

onUnmounted(() => {
  // コンポーネントがアンマウントされたときにリスナーを解除
  window.removeEventListener('resize', checkWindowSize);
});

const transitionName = ref<string>('fadeOut');
const beforeEnter = (el: any) => {
  gsap.set(el, { opacity: 0 });
};

const enter = (el: any, done: () => void) => {
    gsap.to(el, { opacity: 1, onComplete: done});
};

const leave = (el: any, done: () => void) => {
    gsap.to(el, { opacity: 0, onComplete: done});
};

const follow = () => {
    modal_message.value = "Instagramのページへ飛びます。";
    modal_memory.value = "https://www.instagram.com/itsuki_kb_ts";
    openModal();
}

const call = () => {
    modal_message.value = "電話をかけますか？";
    modal_memory.value = "tel:09037955768";
    openModal();
}
</script>
<template>
  <div class="background" ref="background">
    <Modal :showModal="isModalOpen" @update:showModal="closeModal">
            <!-- モーダルの中身 -->
            <p>{{ modal_message }}</p>
            <div class="nuxt-modal-btn">
                <button class="btn true" @click="closeModal" v-if="!modal_memory">OK</button>
                <button class="btn true" @click="flyLink" v-if="modal_memory">YES</button>
                <button class="btn" @click="closeModal" v-if="modal_memory">NO</button>
            </div>
    </Modal>
    <PcBan v-if="isMobile"/>
    <transition
        :name="transitionName"
        @before-enter="beforeEnter"
        @enter="enter"
        @leave="leave"
    >
      <OneShot v-if="isWelcomeBoard" />
    </transition>
    <div class="main">
      <video id="video" src="@/assets/movies/layer_1.mp4" webkit-playsinline playsinline muted>
        <!--
          poster：動画ファイルが利用できない環境で代替表示される画像
          webkit-playsinline：iOS 9までのSafari用インライン再生指定
          playsinline：iOS 10以降のSafari用インライン再生指定
          muted：音声をミュートさせる
          autoplay：動画を自動再生させる
          loop：動画をループさせる
          controls：コントロールバーを表示する
         -->
        <p>動画を再生できる環境ではありません。</p>
      </video>
      <div class="bbs">
        <ul>
          <li v-for="i in 9">ただのフリーランスです</li>
        </ul>
        <ul>
          <li v-for="i in 9">ただのフリーランスです</li>
        </ul>
      </div>
      <div class="bbs2">
        <ul>
          <li v-for="i in 9">デザイナーやってます</li>
        </ul>
        <ul>
          <li v-for="i in 9">デザイナーやってます</li>
        </ul>
      </div>
      <div class="bbs3">
        <ul>
          <li v-for="i in 9">問題児に見えるだけです</li>
        </ul>
        <ul>
          <li v-for="i in 9">問題児に見えるだけです</li>
        </ul>
      </div>
      <div class="bbs4">
        <ul>
          <li v-for="i in 9">実際はただの変人です</li>
        </ul>
        <ul>
          <li v-for="i in 9">実際はただの変人です</li>
        </ul>
      </div>
      <div class="bbs5">
        <ul>
          <li v-for="i in 9">クスリはやってません</li>
        </ul>
        <ul>
          <li v-for="i in 9">クスリはやってません</li>
        </ul>
      </div>
      <div class="bbs6">
        <ul>
          <li v-for="i in 9">wwwwwwwwwww</li>
        </ul>
        <ul>
          <li v-for="i in 9">wwwwwwwwwww</li>
        </ul>
      </div>
      <div class="bbs7">
        <ul>
          <li v-for="i in 9">あははははは</li>
        </ul>
        <ul>
          <li v-for="i in 9">あははははは</li>
        </ul>
      </div>
      <div class="bbs8">
        <ul>
          <li v-for="i in 9">娑婆は暑いよね</li>
        </ul>
        <ul>
          <li v-for="i in 9">娑婆は暑いよね</li>
        </ul>
      </div>
      <div class="my_profile_card">
        <div class="image">
                <img src="@/assets/images/kao.jpg">
        </div>
        <div class="profile_name">
                <ruby>こばしがわ　いつき</ruby>
                <p id="name">小橋川　樹</p>
        </div>
        <div class="iro_iro">
          合同会社マジムンスタジオ　代表社員
        </div>
        <div class="follow_btn">
                <button @click="follow">インスタを見る</button>
        </div>
        <div class="follow_btn2">
                <button @click="call">電話する</button>
                <button @click="contactForm">Contact</button>
        </div>
      </div>
    </div>
  </div>
</template>