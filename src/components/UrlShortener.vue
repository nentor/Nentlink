<template>
  <div class="url-shortener" id="url-shortener">
    <input
      class="url-shortener__input"
      type="text"
      v-model="userUrl"
      placeholder="Enter your URL here"
      @keyup.enter="submit"
    />
    <button ref="button" class="button button--sub" @click="shortenURL">Shorten</button>
  </div>
</template>

<script>
import { eventBus } from "../main";

export default {
  data() {
    return {
      userUrl: ""
    };
  },
  methods: {
    submit() {
      this.$refs["button"].click();
    },
    shortenURL() {
      let regex = /^(http:\/\/|https:\/\/)/;
      let shortenedUrl = "";
      let userUrlCopy = this.userUrl;

      if (!this.userUrl.match(regex)) {
        this.userUrl = "http://" + this.userUrl;
        userUrlCopy = this.userUrl;
      }

      let myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/x-www-form-urlencoded");

      let urlencoded = new URLSearchParams();
      urlencoded.append("url", this.userUrl);

      let requestOptions = {
        method: "POST",
        headers: myHeaders,
        body: urlencoded,
        redirect: "follow"
      };

      fetch("https://rel.ink/api/links/", requestOptions)
        .then(blob => blob.json())
        .then(result => {
          shortenedUrl = "https://" + "rel.ink/" + result.hashid;
          eventBus.$emit("url-added", [userUrlCopy, shortenedUrl]);
        })
        .catch(error => error);

      this.userUrl = ""; // reset
    }
  }
};
</script>

<style></style>
