<template>
  <v-app>
    <Header></Header>

    <v-main>
      <MainPage />
    </v-main>

    <Footer></Footer>
  </v-app>
</template>

<script>
import Header from "./components/Header";
import Footer from "./components/Footer";
import MainPage from "./components/MainPage";

export default {
  name: "App",
  components: {
    Header,
    MainPage,
    Footer,
  },
};
</script>
