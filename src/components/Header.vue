<template>
  <div id="header">
    <v-toolbar dense>
      <v-spacer></v-spacer>

      <v-avatar size="40" class="mr-2">
        <span class="white--text headline">{{ initials }}</span>
      </v-avatar>

      <v-menu top>
        <template v-slot:activator="{ on, attrs }">
          <span v-on="on" id="user_initials">{{ `Hello ${name}` }}</span>
        </template>

        <v-list>
          <v-list-item
            v-for="(item, index) in menu_items"
            :key="index"
            @click=""
          >
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-toolbar>

    <v-card>
      <v-img
        id="background_img"
        :src="background_img_path"
        position="bottom 22% center"
        height="300"
      ></v-img>
    </v-card>

    <v-container grid-list-xl>
      <v-layout row wrap>
        <v-flex xs12 offset-sm6>
          <div id="profile_layer">
            <div id="profile_img">
              <img :src="profile_img_path" />
            </div>
            <div id="profile_name">{{ `${name} ${surname}` }}</div>
          </div>
        </v-flex>
      </v-layout>
    </v-container>
  </div>

  <!--<v-img :src="background_img_path" height="300px" position="bottom center"></v-img>-->
</template>

<script type="text/babel">
export default {
  data: () => ({
    name: "Thrasyvoulos",
    surname: "Kafasis",
    background_img_path: require("@/assets/images/Your_Cover.jpg"),
    profile_img_path: require("@/assets/images/profile-img.jpg"),

    menu_items: [{ title: "Settings" }, { title: "Logout" }],
  }),
  computed: {
    initials() {
      return `${this.name.charAt(0).toUpperCase()}${this.surname
        .charAt(0)
        .toUpperCase()}`;
    },
  },
};
</script>