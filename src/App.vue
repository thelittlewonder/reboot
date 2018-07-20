<template>
  <div id="app">
    <Main></Main>
  </div>
</template>

<script>
import Main from "./components/Main";
import "reset-css";

export default {
  name: "app",
  components: {
    Main
  }
};
</script>

<style>
html {
  overflow: hidden;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
