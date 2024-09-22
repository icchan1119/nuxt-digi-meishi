<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

const isDescription = ref(false);
const description = ref(null);
const postCount = ref(0);

const isModalOpen = ref(false);


</script>
<template>
    <div class="profile">
        <div id="header">
            ナニモノ？
        </div>
        <div id="main">
            <div class="image">
                <img src="../assets/images/mslogo.jpg">
            </div>
            <div class="mvv">
                <h1>革新的なクリエイティブで<br>ITをもっと強く</h1>
                <p class="description">
                    マジムンスタジオ代表<br>
                    小橋川　樹です。<br>
                    弊社は主にWebシステムの構築など
                    <br>を受託で行っております。<br>
                    他にも今後Webサイト・アプリケーション開発の<br>下流工程から上流工程まで<br>
                    行ってきた経験をもとに<br>ITクリエイティブスクールを開講いたします。<br>
                    また、クリエイティブスクールと共に、<br>エンジニア同士で助け合っていき<br>一人の「マジムン」を増やしていけるよう<br>コミュニティも開設いたします！！
                </p>
            </div>
        </div>
    </div>
</template>