<template>
  <v-container grid-list-xl id="main_page" class="mt-5">
    <v-row dense>
      <!-- Search -->
      <v-col cols="12" md="3" sm="4" offset-md="6" offset-sm="4">
        <v-text-field
          label="Search Channels"
          v-model="search_value"
          solo
          prepend-inner-icon="search"
          clearable
        ></v-text-field>
      </v-col>

      <!-- Categories -->
      <v-col cols="12" md="3" sm="4">
        <v-select
          label="Search category"
          :items="categories"
          v-model="selected_category"
          solo
          clearable
          @change="categoryChanged"
        ></v-select>
      </v-col>

      <v-divider></v-divider>

      <!-- Sources -->
      <v-col
        cols="12"
        sm="4"
        v-for="(source, index) in source_items"
        :key="index"
      >
        <v-card>
          <v-img
            src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
            height="200px"
          ></v-img>

          <v-card-title> {{ source.name }} </v-card-title>

          <v-card-subtitle> {{ source.description }} </v-card-subtitle>

          <v-card-actions>
            <v-btn text icon>
              <v-icon>local_offer</v-icon>
            </v-btn>
            {{ source.category }}
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>

    <v-row dense>
      <!-- Pagination -->
      <v-col cols="12">
        <div id="pagination_layer">
          <div class="item pr-3">Displaying Page</div>

          <!-- Prev page -->
          <div class="item" v-if="pagination.current_page > 1">
            <v-btn text icon @click="prevPage">
              <v-icon>arrow_left</v-icon>
            </v-btn>
          </div>

          <!-- Current page -->
          <div class="item">
            <v-text-field
              v-model="pagination.current_page"
              type="number"
              min="1"
              solo
              @input="paginationChange()"
            >
            </v-text-field>
          </div>

          <!-- Total pages -->
          <div class="item">of {{ totalPages }}</div>

          <!-- Next page -->
          <div class="item" v-if="pagination.current_page < totalPages">
            <v-btn text icon @click="nextPage">
              <v-icon>arrow_right</v-icon>
            </v-btn>
          </div>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import _ from "loadsh";

export default {
  data: () => ({
    sources: [],
    source_items: [],
    pagination: {
      current_page: 1,
      items_per_page: 6,
    },

    // search channel
    search_value: null,

    // search by category
    categories: [],
    selected_category: null,
  }),
  mounted() {
    this.getData();
  },
  computed: {
    totalPages() {
      return Math.ceil(this.sources.length / this.pagination.items_per_page);
    },
  },
  watch: {
    search_value: _.debounce(function (val) {
      // clone initial array
      this.sources = [...this.initial_sources];

      // reset pagination current page
      this.pagination.current_page = 1;

      // a category selected
      if (val) {
        let up_val = val.toUpperCase();
        this.sources = this.sources.filter((item) =>
          item.name.toUpperCase().includes(up_val)
        );
      }

      this.paginateItems();
    }, 300),
  },
  methods: {
    // get data from api
    async getData() {
      let response = await axios.get("https://newsapi.org/v1/sources");
      this.initial_sources = response.data.sources;
      this.sources = response.data.sources;
      this.paginateItems();

      // get categories
      this.categories = [...new Set(this.sources.map((item) => item.category))];
    },
    // paginate items
    paginateItems() {
      let start =
        (this.pagination.current_page - 1) * this.pagination.items_per_page;
      let end = this.pagination.current_page * this.pagination.items_per_page;

      this.source_items = this.sources.slice(start, end);
    },
    // prev page click button
    prevPage() {
      if (this.pagination.current_page > 1) {
        this.pagination.current_page--;
        this.paginateItems();
      }
    },
    // next page click button
    nextPage() {
      if (this.pagination.current_page < this.totalPages) {
        this.pagination.current_page++;
        this.paginateItems();
      }
    },
    // change on pagination field
    paginationChange() {
      if (
        this.pagination.current_page >= 1 &&
        this.pagination.current_page <= this.totalPages
      ) {
        this.paginateItems();
      }
    },
    // change on selected category
    categoryChanged() {
      // clone initial array
      this.sources = [...this.initial_sources];

      // reset pagination current page
      this.pagination.current_page = 1;

      // a category selected
      if (this.selected_category) {
        this.sources = this.sources.filter(
          (item) => item.category === this.selected_category
        );
      }

      this.paginateItems();
    },
    searchChannel() {},
  },
};
</script>

<style>
</style>
