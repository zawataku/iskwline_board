<script setup>
import html2canvas from 'html2canvas';
import { ref } from 'vue';

const name_kanji = ref('');
const name_hiragana = ref('');

const generate = () => {
  const element = document.querySelector("#preview_inner");
  html2canvas(element, {
    scale: 2, // 高解像度にするためにスケールを調整
    useCORS: true // クロスオリジンリソースをキャプチャするために必要な場合
  }).then(function (canvas) {
    var result = document.querySelector(".result");
    if (result) {
      result.innerHTML = '';
      result.appendChild(canvas);
    } else {
      console.error('Result element not found');
    }
  });
};
</script>

<template>
  <div class="text-center text-2xl font-bold">
    <h1>Welcome to</h1>
    <h1>北陸鉄道石川線 駅名標ジェネレータ</h1>
  </div>

  <div id="preview" class="block">
    <div id="preview_inner" class="w-96 h-48 mx-auto relative flex items-center justify-center">
      <div id="name_hiragana" class="absolute inset-0 top-8 flex justify-center text-center text-4xl">{{ name_hiragana
        }}</div>
      <div id="name_kanji" class="absolute inset-0 top-20 flex justify-center text-center text-base">{{ name_kanji }}
      </div>
      <img src="./assets/board_base.jpg" class="w-full h-full object-cover" alt="">
    </div>
  </div>
  <div class="w-2/3 mx-auto my-5">
    <label class="input input-bordered flex items-center gap-2">
      <input type="text" class="grow" placeholder="駅名（漢字）" v-model="name_kanji" />
    </label>
    <label class="input input-bordered flex items-center gap-2">
      <input type="text" class="grow" placeholder="駅名（ひらがな）" v-model="name_hiragana" />
    </label>
    <label class="input input-bordered flex items-center gap-2">
      <input type="text" class="grow" placeholder="駅名（ローマ字）" v-model="name_rome">
    </label>
    <label class="input input-bordered flex items-center gap-2">
      <input type="text" class="grow" placeholder="次の駅名（ひらがな）" v-model="next_name_hiragana" />
    </label>
    <label class="input input-bordered flex items-center gap-2">
      <input type="text" class="grow" placeholder="次の駅名（ローマ字）" />
    </label>
    <label class="input input-bordered flex items-center gap-2">
      <input type="text" class="grow" placeholder="前の駅名（ひらがな）" />
    </label>
    <label class="input input-bordered flex items-center gap-2">
      <input type="text" class="grow" placeholder="前の駅名（ローマ字）" />
    </label>
  </div>

  <button class="btn-outline" @click="generate">Generate</button>

  <div class="result"></div>
</template>
