<template>
  <v-footer height="auto">
    <v-container grid-list-xl>
      <v-layout row wrap>
        <div class="item">Copyright Creative Workers</div>
        <div class="item">
          <v-btn text rounded href="https://www.google.com" target="_blank"
            >Privacy</v-btn
          >
          <span>-</span>
          <v-btn text rounded href="https://www.google.com" target="_blank"
            >Terms</v-btn
          >
          <span>-</span>
          <v-btn text rounded href="https://www.google.com" target="_blank"
            >Cookies</v-btn
          >
          <span>-</span>
          <v-btn text rounded href="https://www.google.com" target="_blank"
            >More</v-btn
          >
        </div>
        <div class="item">Kissmybutton &copy;2017</div>
      </v-layout>
    </v-container>
  </v-footer>
</template>

<script>
export default {
  data: () => ({
    links: ["Home", "About Us", "Team", "Services", "Blog", "Contact Us"],
  }),
};
</script>