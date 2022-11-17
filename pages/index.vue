<template>
  <div class="grid">
    <header>
      <h4>PHP.ini generator</h4>
      <dm-toast v-if="toast" width="400px" color="green"
        >Yay, you have successfully copied the text!</dm-toast
      >
    </header>
    <!-- </div> -->
    <div class="left side" @click="showButtons">
      <dm-input
        name="maxinputvars"
        :validation="'between:3000,9000'"
        validation-vv-as="vars"
        size="mini"
        :full-width="false"
        label="max input vars"
        v-model.trim="vars"
      ></dm-input>
      <dm-input
        name="maxfilesize"
        size="mini"
        :validation="'numeric'"
        validation-vv-as="max filesize"
        label="upload max filesize"
        v-model="fileSize"
      ></dm-input>
      <dm-input
        name="maxexecutiontime"
        size="mini"
        :validation="'numeric'"
        validation-vv-as="execution time"
        label="max execution time"
        v-model="maxExecution"
      ></dm-input>
      <dm-input
        name="postmaxsize"
        :validation="'numeric'"
        validation-vv-as="post max size"
        size="mini"
        label="post max size"
        v-model="postMaxSize"
      ></dm-input>
    </div>
    <!-- end of left side -->
    <div class="right side">
      <div class="show">
        <transition name="slide-fade">
          <p v-if="!isNaN(vars) && vars != ''">max_input_vars = {{ vars }}M</p>
        </transition>
        <transition name="slide-fade">
          <p v-if="!isNaN(fileSize) && fileSize != ''">
            upload_max_filesize = {{ fileSize }}M
          </p>
        </transition>
        <transition name="slide-fade">
          <p v-if="!isNaN(maxExecution) && maxExecution != ''">
            max_execution_time= {{ maxExecution }}
          </p>
        </transition>
        <transition name="slide-fade">
          <p v-if="!isNaN(postMaxSize) && postMaxSize != ''">
            post_max_size = {{ postMaxSize }}M
          </p>
        </transition>
      </div>
      <transition name="slide-fade">
        <div class="buttons" v-if="!isActive">
          <dm-button
            @click="onClick"
            size="medium"
            left-icon="file_copy"
            color="green"
            >Copy code</dm-button
          >
          <span>&nbsp;OR&nbsp;</span>
          <a
            @click="onDownload"
            id="download_link"
            download="php.ini"
            href
            class="dm-base-button dm-base-button--green dm-base-button--medium"
          >
            <span class="dm-base-button__inner">
              <i
                aria-hidden="true"
                class="dm-base-button__left-icon dm-base-icon"
                >cloud_download</i
              >
              <span class="dm-base-button__label"
                >Download as php.ini file</span
              >
            </span>
          </a>
        </div>
      </transition>
      <div v-if="isActive">
        <br />
        <dm-list-item :number="1" color="green"
          >Add your values to the inputs on the left.</dm-list-item
        >
        <dm-list-item :number="2" color="green"
          >Copy or download the php.ini file and upload to your
          server.</dm-list-item
        >
      </div>
    </div>
    <!-- end of right side -->
    <footer>
      <small>
        Made with
        <a href="https://nuxtjs.org/">nuxt.js</a> by
        <b>Ivo Culic</b>
      </small>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      toast: false,
      isActive: true,
      vars: "",
      fileSize: "",
      maxExecution: "",
      postMaxSize: "",
      emptyText:
        "Hey, this is empty now! You must first add value to at least one of the inputs!",
    };
  },
  methods: {
    onClick: function () {
      let copyText = document.querySelector(".show").innerText;
      this.$copyText(copyText).then(
        (this.toast = true),
        function (e) {
          // alert("Yay, you have successfully copied the text!");
          console.log(e);
        },
        function (e) {
          if (copyText !== "") {
            alert("Can not copy");
            console.log(e);
          }
        }
      );
    },
    onDownload: function (e) {
      let text = "";
      if (
        this.vars !== "" ||
        this.fileSize !== "" ||
        this.maxExecution !== ""
      ) {
        text = document.querySelector(".show").innerText;
      } else {
        text = this.emptyText;
      }
      let data = new Blob([text], { type: "text/.ini" });
      let url = window.URL.createObjectURL(data);
      document.getElementById("download_link").href = url;
    },
    showButtons: function () {
      this.isActive = false;
    },
  },
};
</script>

<style scoped>
.slide-fade-enter-active {
  transition: all 0.9s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
.grid {
  background: #2b2c34;
  width: 850px;
  display: grid;
  grid-template-rows: auto;
  grid-template-columns: repeat(4, 1fr);
  grid-template-areas:
    "header header header header"
    "left right right right"
    "footer footer footer footer";
  grid-gap: 0;
  -webkit-box-shadow: 5px 5px 15px 5px rgba(28, 37, 51, 0.44);
  box-shadow: 5px 5px 15px 5px rgba(28, 37, 51, 0.44);
  z-index: 9;
}
a.dm-base-button {
  text-decoration: none;
}
.dm-base-list-item {
  color: #fff;
  margin-top: 15px;
}
.dm-base-list-item--green
  .dm-base-list-item__container
  .dm-base-list-item__circle {
  border-color: #399549 !important;
}
.dm-base-list-item .dm-base-list-item__container .dm-base-list-item__circle {
  width: 25px !important;
  height: 25px !important;
}
.show {
  box-sizing: border-box;
  border-width: 1px;
  border-style: solid;
  border-radius: 6px;
  background-color: #283243;
  display: flex;
  flex-direction: column;
  text-align: left;
  min-block-size: 350px;
  color: #fff;
  padding: 15px;
  border: 1px solid #323e4f;
}
h1,
h2,
h3,
h4 {
  color: #fff;
}
header {
  background: #272830;
  grid-area: header;
  padding: 10px 0 10px 30px;
  text-align: left;
}
.left {
  grid-area: left;
}
.right {
  background: #1a1a1e;
  grid-area: right;
}
.side {
  padding: 70px 30px;
}
footer {
  background: #399549;
  font-size: 16px;
  color: #fff;
  padding: 5px 0;
  grid-area: footer;
}
footer a {
  font-weight: bold;
  color: #fff;
  text-decoration: none;
}
footer a:hover {
  text-decoration: underline;
}
.buttons {
  margin-top: 50px;
}
.dm-field-input {
  margin-bottom: 30px;
}
.dm-field-input--focused .dm-field-input__container {
  border-color: #399549 !important;
}
[class^="dm-"]:not(.dm-base-icon) {
  font-family: "Nunito", sans-serif !important;
  font-size: 17px;
}
[class^="gb-"]:not(.gb-base-icon),
.dm-base-heading {
  font-family: "Nunito", sans-serif !important;
}
.dm-base-button--green:not(.dm-base-button--reverse) {
  background: #399549;
}
span {
  color: #fff;
}
</style>
