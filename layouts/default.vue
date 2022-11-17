<template>
  <div
    class="container"
    v-bind:style="{ backgroundImage: 'url(' + bg_images.bg_image + ')' }"
  >
    <Nuxt />
    <div class="overlay"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bg_images: "",
    };
  },
  methods: {},
  mounted() {
    fetch(
      "https://gist.githubusercontent.com/ifkas/275169ac7efedf380dc6f77e2f7c8266/raw/93b3b84c43f58832698ba7e5326e5294f7d33b80/bg_images.json"
    ).then((response) => {
      response.json().then((bg_images) => {
        const keys = Object.keys(bg_images);
        const randIndex = Math.floor(Math.random() * keys.length);
        const randKey = keys[randIndex];
        this.bg_images = bg_images[randKey];
        // console.log(this.bg_images.bg_image);
      });
    });
  },
};
</script>

<style>
html {
  font-family: "Nunito", sans-serif !important;
  font-size: 18px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
  color: #2E495E;
  background: #1C2533;
  background-size: cover;
}
.overlay {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  background: rgba(0, 0, 0, 0.2);
  z-index: 1;
}
*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  background-size: cover;
}
.left.side {
  position: relative;
}
.dm-field-message .dm-field-message__icon,
.dm-field-input .dm-field-input__container .dm-field-input__icon {
  display: none !important;
}
.dm-field-message--error {
  color: #fff !important;
}
.dm-field-label {
  font-family: "Nunito", sans-serif !important;
}
/* .dm-field-message {
  position: absolute;
  left: -100px;
  width: 150px;
  background: #283243;
} */
.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #00C48D;
  color: #00C48D;
  text-decoration: none;
  padding: 10px 30px;
  display: inline-flex;
  align-self: center;
}
.button--green:hover {
  color: #fff;
  background-color: #00C48D;
}
.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #2E495E;
  color: #2E495E;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}
.button--grey:hover {
  color: #fff;
  background-color: #2E495E;
}
.dm-base-heading {
  font-family: "Nunito", sans-serif !important;
}
</style>
