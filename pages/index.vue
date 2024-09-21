<script setup lang="ts">
import { Ref } from 'vue';
import { ref, onMounted, onUnmounted } from 'vue';
// import { Particles } from 'nuxt-particles';

const isMobile: Ref<boolean> = ref(false);
const isWelcomeBoard: Ref<boolean> = ref(true);
const { $gsap: gsap } = useNuxtApp();
const { $swal: swal} = useNuxtApp();
const modal_message: Ref<string> = ref("");
const modal_memory: Ref<string> = ref("");

const trueProfile: Ref<boolean> = ref(false);

const backButton: Ref<boolean> = ref(false);

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

const checkActivity = (): void => {
    trueProfile.value = true;
    backButton.value = true;
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
          setTimeout(() => {
              gsap.to('.my_profile_card',{ opacity: 1, y:-100});
            }, 500);
        }, 3500);
    }
});

onUnmounted(() => {
  // コンポーネントがアンマウントされたときにリスナーを解除
  window.removeEventListener('resize', checkWindowSize);
});

const transitionName = ref<string>('fadeOut');
const transitionProfileName = ref<string>('proFadeIn');
const beforeEnter = (el: any) => {
  gsap.set(el, { opacity: 0 });
};

const hideProfile = () => {
  trueProfile.value = false;
  backButton.value = false;
}

const enter = (el: any, done: () => void) => {
    gsap.to(el, { opacity: 1, onComplete: done});
};

const leave = (el: any, done: () => void) => {
    gsap.to(el, { opacity: 0, onComplete: done});
};

const proEnter = (el: any, done: () => void) => {
    gsap.to(el, { opacity: 1,scaleX:1, scaleY:1, duration: 0.2, onComplete: done});
};

const proLeave = (el: any, done: () => void) => {
    gsap.to(el, { opacity: 0, scaleX:0.7, scaleY:0.7, duration: 0.3, onComplete: done});
};

const buttonEnter = (el: any, done: () => void) => {
    gsap.to(el, { opacity: 1,scaleX:1, scaleY:1, duration: 0.2, onComplete: done});
};

const buttonLeave = (el: any, done: () => void) => {
    gsap.to(el, { opacity: 0, y: 100, duration: 0.3, onComplete: done});
};

const follow = () => {
    swal.fire({
      title: "💬📸💬",
      text: "Instagramのページへ移動します。",
      showCancelButton: true,
      confirmButtonColor: "#3085d6",
      cancelButtonColor: "#d33",
      confirmButtonText: "はい",
      cancelButtonText: "いいえ"
    }).then((result) => {
      /* Read more about isConfirmed, isDenied below */
      if (result.isConfirmed) {
        location.href="https://instagram.com/itsuki_kb_ts";
      } 
    });
}

const call = () => {
    swal.fire({
      title: "🤙〰️📱",
      text: "電話をかけますか？",
      showCancelButton: true,
      confirmButtonColor: "#3085d6",
      cancelButtonColor: "#d33",
      confirmButtonText: "はい",
      cancelButtonText: "いいえ"
    }).then((result) => {
      /* Read more about isConfirmed, isDenied below */
      if (result.isConfirmed) {
        location.href="tel:09037955768";
      } 
    });
}

const remember = () => {
  swal.fire({
      title: "📩👉🧠💾",
      text: "メールアドレスを入力してください。",
      input: "text",
      inputAttributes: {
        autocapitalize: "off"
      },
      preConfirm: async (email) => {
      try {
        const githubUrl = `
          https://hyperform.jp/api/WUzZC85a
        `;
        const response = await fetch(githubUrl,{
          method: 'POST',
          mode: "cors", // no-cors, *cors, same-origin
          cache: "default", // *default, no-cache, reload, force-cache, only-if-cached
          credentials: "same-origin", // include, *same-origin, omit
          headers: {
              'Content-Type': 'application/json'
          },
          body: JSON.stringify({
              email: email
          })
        });
        if (!response.ok) {
          return swal.showValidationMessage(`
            ${JSON.stringify(await response.json())}
          `);
        }
        return response.json();
      } catch (error) {
        swal.showValidationMessage(`
          エラーが発生しました。
        `);
      }
  },
      confirmButtonColor: "#3085d6",
      cancelButtonColor: "#d33",
      confirmButtonText: "送信",
    }).then(async (result) => {
      /* Read more about isConfirmed, isDenied below */
      if (result.isConfirmed) {
        swal.fire({
          title: `${result.value.login}'s avatar`,
          imageUrl: result.value.avatar_url
        });
      } 
  });
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
      <div class="my_profile_card">
        <div class="image">
                <img src="@/assets/images/kao.jpg">
        </div>
        <div class="profile_name">
                <ruby>こばしがわ　いつき</ruby>
                <p id="name">小橋川　樹</p>
        </div>
        <div class="iro_iro">
          マジムンスタジオ　代表
        </div>
        <div class="follow_btn">
                <button @click="follow">つながる</button>
        </div>
        <div class="follow_btn15">
                <button @click="checkActivity">何者か見る</button>
        </div>
        <div class="follow_btn19">
                <button @click="call">電話をかける</button>
        </div>
        <div class="follow_btn24">
                <button @click="remember">おぼえる</button>
        </div>
      </div>
      <transition
        :name="transitionName"
        @before-enter="beforeEnter"
        @enter="proEnter"
        @leave="proLeave"
      >
        <Profile v-if="trueProfile" />
      </transition>
      <transition
        :name="transitionName"
        @before-enter="beforeEnter"
        @enter="buttonEnter"
        @leave="buttonLeave"
      >
        <button class="back-button" @click="hideProfile" v-if="backButton">
          <img src="../assets/images/home.svg">
        </button>
      </transition>
    </div>
  </div>
</template>