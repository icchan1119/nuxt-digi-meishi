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
                <h1>革新的なクリエイティブで<br>IT市場をもっと強く</h1>
                <p class="description">
                    ども<br>
                    マジムンスタジオ代表<br>
                    小橋川　樹です。
                </p>
            </div>
        </div>
    </div>
</template>