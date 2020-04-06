<template>
  <div class="urlList">
    <ul class="urlList__list">
      <li class="urlList__item" v-for="(urlObj, index) in urlList" :key="index">
        <span class="urlList__original">{{ urlObj.originalUrl }}</span>
        <span class="urlList__shortened">
          <a
            class="urlList__link"
            :href="urlObj.shortenedUrl"
            target="_blank"
            ref="shortenedUrl"
          >{{ urlObj.shortenedUrl }}</a>
        </span>
        <a @click="copyToClipboard" class="urlList__copy">
          <i class="far fa-copy"></i>
        </a>
        <a class="urlList__remove" @click="removeUrl(urlObj)">&#215;</a>
      </li>
    </ul>
  </div>
</template>

<script>
import { eventBus } from "../main";

export default {
  // props: ["url"],
  data() {
    return {
      urlList: []
    };
  },
  methods: {
    removeUrl(urlObj) {
      this.urlList.splice(this.urlList.indexOf(urlObj), 1);
    },
    copyToClipboard() {
      const copyUrl = this.$refs.shortenedUrl[0];

      const dummyTextArea = document.createElement("textarea");
      dummyTextArea.value = copyUrl;
      document.body.appendChild(dummyTextArea);
      dummyTextArea.select();

      document.execCommand("copy");
      document.body.removeChild(dummyTextArea);
    }
  },
  mounted() {
    eventBus.$on("url-added", urlArr => {
      this.urlList.unshift({
        originalUrl: urlArr[0],
        shortenedUrl: urlArr[1]
      });
    });

    if (JSON.parse(localStorage.getItem("urlListCached"))) {
      this.urlList = JSON.parse(localStorage.getItem("urlListCached"));
    }
  },
  beforeUpdate() {
    localStorage.setItem("urlListCached", JSON.stringify(this.urlList));
  }
};
</script>

<style></style>
