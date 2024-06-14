<template>
  <div id="app">
    <div class="inner">
      <div class="header mb-4">
        <h1 class="header-inner">北鉄石川線 駅名標ジェネレータ</h1>
      </div>
      <div class="main">
        <canvas class="canvas" ref="canvas" width="1280" height="720"></canvas>
        <img class="output-image" ref="outputImage" :src=imageDataUrl alt="Generated Image" />
      </div>

      <div class="forms">
        <p>右クリックメニュー「名前を付けて画像を保存」またはロングタップで保存できます</p>
        <h3 class="mt-4">駅の設定</h3>
        <input class="form-control my-2 mx-auto" v-model="text0" placeholder="駅名（ひらがな）" />
        <input class="form-control my-2 mx-auto" v-model="text1" placeholder="駅名（漢字）" />
        <input class="form-control my-2 mx-auto" v-model="text2" placeholder="駅名（ローマ字）" />
      </div>

      <div class="forms">
        <h3 class="mt-4">前の駅の設定</h3>
        <input class="form-control my-2 mx-auto" v-model="text3" placeholder="駅名（ひらがな）" />
        <input class="form-control my-2 mx-auto" v-model="text4" placeholder="駅名（ローマ字）" />
      </div>

      <div class="forms">
        <h3 class="mt-4">次の駅の設定</h3>
        <input class="form-control my-2 mx-auto" v-model="text5" placeholder="駅名（ひらがな）" />
        <input class="form-control my-2 mx-auto" v-model="text6" placeholder="駅名（ローマ字）" />
      </div>

      <div class="footer mt-5">
        <p>Version 1.0.0</p>
        <p>Licensed under the MIT License</p>
        <a href="https://github.com/zawataku/iskwline_board">GitHub Repository</a>
      </div>
    </div>
  </div>
</template>

<script>
import html2canvas from 'html2canvas';

export default {
  data() {
    return {
      text0: 'ののいちこうだいまえ',
      text1: '野々市工大前',
      text2: 'Nonoichi kōdaimae',
      text3: 'まがえ',
      text4: 'Magae',
      text5: 'ののいち',
      text6: 'Nonoichi',
      image: null,
      fontsLoaded: false,
      imageDataUrl: '/images/loading.webp'
    };
  },
  watch: {
    text0() {
      this.drawText();
    },
    text1() {
      this.drawText();
    },
    text2() {
      this.drawText();
    },
    text3() {
      this.drawText();
    },
    text4() {
      this.drawText();
    },
    text5() {
      this.drawText();
    },
    text6() {
      this.drawText();
    },
  },
  mounted() {
    this.loadResources();
  },
  methods: {
    async loadResources() {
      await this.loadFonts();
      await this.loadImage();
      this.drawText();
    },
    loadFonts() {
      return new Promise((resolve) => {
        const font0 = new FontFace('Font_0', 'url(/fonts/GENJYUUGOTHICX-BOLD.woff2)');
        Promise.all([font0.load()]).then((loadedFonts) => {
          loadedFonts.forEach((font) => document.fonts.add(font));
          this.fontsLoaded = true;
          resolve();
        });
      });
    },
    loadImage() {
      return new Promise((resolve) => {
        const img = new Image();
        img.src = '/images/board_base.png'; // 画像のパスを指定
        img.onload = () => {
          this.image = img;
          resolve();
        };
      });
    },
    drawText() {
      if (!this.fontsLoaded || !this.image) return; // フォントと画像がロードされるのを待つ

      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext('2d');
      ctx.clearRect(0, 0, canvas.width, canvas.height); // キャンバスをクリア
      if (this.image) {
        ctx.drawImage(this.image, 0, 0, canvas.width, canvas.height); // 画像を描画
      }
      ctx.textAlign = 'center'; // テキストを中央揃えに設定
      const x = canvas.width / 2; // キャンバスの幅の中央

      // テキスト0を描画
      ctx.font = '125px Font_0'; // ここでフォントを変更
      ctx.fillStyle = 'black';
      const y0 = 200;
      ctx.fillText(this.text0, x, y0); // テキストを描画

      // テキスト1を描画
      ctx.font = '80px Font_0';
      ctx.fillStyle = 'black';
      const y1 = 350;
      ctx.fillText(this.text1, x, y1); // テキストを描画

      // テキスト2を描画
      ctx.font = '60px Font_0'; // ここでフォントを変更
      ctx.fillStyle = 'white';
      const y2 = 470;
      ctx.fillText(this.text2, x, y2); // テキストを描画

      // テキスト3を描画
      ctx.font = '60px Font_0'; // ここでフォントを変更
      ctx.fillStyle = 'black';
      const y3 = 600;
      const x2 = 200;
      ctx.fillText(this.text3, x2, y3); // テキストを描画

      // テキスト4を描画
      ctx.font = '40px Font_0'; // ここでフォントを変更
      ctx.fillStyle = 'black';
      const y4 = 660;
      ctx.fillText(this.text4, x2, y4); // テキストを描画

      // テキスト5を描画
      ctx.font = '60px Font_0'; // ここでフォントを変更
      ctx.fillStyle = 'black';
      const x3 = 1020;
      ctx.fillText(this.text5, x3, y3); // テキストを描画

      // テキスト6を描画
      ctx.font = '40px Font_0'; // ここでフォントを変更
      ctx.fillStyle = 'black';
      const y6 = 660;
      ctx.fillText(this.text6, x3, y4); // テキストを描画

      this.imageDataUrl = canvas.toDataURL('image/png');
    },
  },
};
</script>
