<template>
  <div id="app" class="bg-gray-200 h-screen w-screen">
    <Header />
    <Main
      :changeSeatBtnClicked="changeSeatBtnClicked"
      @reset-btn-flag="changeSeat"
      :config="config"
    />
    <Footer @change-seat="changeSeat" @open-config="openConfig" />
  </div>
</template>

<script>
import Header from "@/global/Header";
import Main from "@/pages/Main";
import Footer from "@/global/Footer";

export default {
  name: "App",
  data() {
    return {
      changeSeatBtnClicked: false,
      openedConfig: false,
      config: {
        cols: 4,
        seats: 24,
      },
    };
  },
  components: {
    Header,
    Main,
    Footer,
  },
  methods: {
    changeSeat() {
      this.changeSeatBtnClicked = !this.changeSeatBtnClicked;
    },
    openConfig(event) {
      this.config = event;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
