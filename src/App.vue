<template>
  <div id="app">
    <app-header></app-header>
    <main class="main">
      <h2 class="heading-secondary">URL Shortener</h2>
      <app-url-shortener></app-url-shortener>
      <app-url-list></app-url-list>
    </main>
    <app-footer></app-footer>
  </div>
</template>

<script>
import Header from "./components/Header";
import UrlShortener from "./components/UrlShortener";
import UrlList from "./components/UrlList";
import Footer from "./components/Footer";

export default {
  name: "App",
  data() {
    return {};
  },
  components: {
    appHeader: Header,
    appUrlShortener: UrlShortener,
    appUrlList: UrlList,
    appFooter: Footer
  }
};
</script>
