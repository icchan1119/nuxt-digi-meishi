<script lang="ts" setup>
import { ref,onMounted } from 'vue';
// デプロイテスト
const piliod = ref<string>('');
const load = ref<number>(0);
onMounted(() => {
  setInterval(() => {
    load.value += 1;
    piliod.value += '.';
    if (load.value == 4){
      piliod.value = '';
      load.value = 0;
    }
  },200)
})
</script>

<template>
  <div id="root">
    <div id="load">
      <p>Now Loading</p>
    </div>
  </div>
</template>

<style lang="scss" scoped>
  @font-face {
    font-family: 'dotfont'; /* フォント名 */
    /* フォーマットごとにパスを指定 */
    src: url('~/assets/fonts/dotfont.ttf') format('truetype');
  }
  #root{
    background-color: #000000;
    width: 100%;
    height: 100vh;
    position: relative;

    #load {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 40vh;
      margin: auto;
      width: 182px;
      height: 40px;

      img {
        margin-left: 17px;
        width: 150px;
        border-radius: 100%;
      }

      p {
        margin-top: 90px;
        color: #fff;
        font-family: 'dotfont', sans-serif;
        font-size: 13px;
      }
    }
  }
</style>
