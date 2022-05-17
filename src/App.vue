<template>
  <v-app>
    <v-main id="main" class="black">
      <router-view :firmenJson="firmenJson" />
    </v-main>
  </v-app>
</template>
<script>
import axios from 'axios';
export default {
  name: 'App',

  data: () => ({
    // firmen: [],
    firmenJson: [],
  }),
  created() {
    // this.getFirmen();
    this.getFirmenJson();
  },
  methods: {
    // async getFirmen() {
    //   const { data } = await axios({
    //     url: '/firmen',
    //     method: 'GET',
    //   });
    //   this.firmen = data;
    // },
    async getFirmenJson() {
      const { data } = await axios({
        url: '/firmenJson',
        method: 'GET',
      });
      this.firmenJson = data;
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Titillium+Web:wght@200;300;400;600;700&display=swap');

html * {
  font-family: 'Titillium Web', sans-serif;
  scroll-behavior: smooth;
  font-size: 35px;
}
html {
  scroll-behavior: smooth;
}
body *,
body {
  scroll-behavior: smooth;
}
</style>
