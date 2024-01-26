<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

const isDescription = ref(false);
const description = ref(null);
const postCount = ref(0);
const modal_message = <string>ref("");
const modal_memory = <string>ref("");

const isModalOpen = ref(false);

const openModal = () => {
      isModalOpen.value = true;
};

const closeModal = () => {
    isModalOpen.value = false;
};

const { $gsap: gsap } = useNuxtApp();

const beforeEnter = (el) => {
  el.style.opacity = 0;
};

const flyLink = () => {
    location.href = modal_memory.value;
}

const enter = (el, done) => {
    gsap
        .fromTo(
            el, 
            { opacity: 0, y:-40, onComplete: done },
            { opacity: 1, y:0, onComplete: done }
        );
    
    gsap.
        to(
            '.arrowUp',
            { rotation: 180, onComplete: done }
        );
};

const leave = (el, done) => {
  gsap.to(el, { opacity: 0, y:-40, duration: 0.5, onComplete: done });
  gsap.
        to(
            '.arrowUp',
            { rotation: 0, onComplete: done }
        );
};

const openDescription = () => {
    console.log(description.value.style.height);
    if(isDescription.value == true){
        isDescription.value = false;
    } else {
        isDescription.value = true;
    }
}

const contactForm = () => {
    modal_memory.value = "";
    modal_message.value = "この機能はまだ完成していません。";
    openModal();
}

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
    <div class="profile">
        <div id="header">
            (仮) 合同会社沖縄新地
        </div>
        <div id="main">
            <div class="image">
                <img src="@/assets/images/kao.jpg">
            </div>
            <div class="profile_name">
                <ruby>こばしがわ　いつき</ruby>
                <p id="name">小橋川 樹</p>
            </div>
            <div class="follow_btn">
                <button @click="follow">フォローしていく</button>
            </div>
            <div class="follow_btn2">
                <button @click="call">電話する</button>
                <button @click="contactForm">Contact</button>
            </div>
        </div>
        <div id="descriptions" @click="openDescription" ref="description">
            <p>詳細</p>
            <div class="description">
                <transition
                    :name="transitionName"
                    @before-enter="beforeEnter"
                    @enter="enter"
                    @leave="leave"
                >
                    <div class="description_txt" v-if="isDescription">
                        初めまして。<br>
                        小橋川と申します。<br><br>
                        大学生 × Webエンジニア<br><br>
                        Webサイト制作以外にも、Webデザイン、<br>画像編集、動画編集など可能です。<br>
                        依頼等はインスタグラムのDMからお願いします。
                    </div>
                </transition>
                <font-awesome-icon icon="chevron-down" ref="arrowUp" size="1x" class="arrowUp" />
            </div>
            <div class="followers">
                <div class="dontknow">
                    <b>知らない人</b>
                    <p>1.0M人</p>
                </div>
                <div class="know">
                    <b>知ってる人</b>
                    <p>100人</p>
                </div>
            </div>
            <div class="post">
                <ul v-if="postCount != 0"></ul>
                <div class="postNotFound" v-else-if="postCount == 0">
                    <div class="icon-circle">
                        <font-awesome-icon icon="camera" size="3x" class="icon"/>
                    </div>
                    <div class="notfoundtext">
                        新参者なので<br>
                        投稿はありません
                    </div>
                </div>
            </div>
        </div>
        <Modal :showModal="isModalOpen" @update:showModal="closeModal">
            <!-- モーダルの中身 -->
            <p>{{ modal_message }}</p>
            <div class="nuxt-modal-btn">
                <button class="btn true" @click="closeModal" v-if="!modal_memory">OK</button>
                <button class="btn true" @click="flyLink" v-if="modal_memory">YES</button>
                <button class="btn" @click="closeModal" v-if="modal_memory">NO</button>
            </div>
        </Modal>
    </div>
</template>