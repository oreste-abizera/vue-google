<template>
  <div>
    <div class="header">
      <div class="pl-10 pr-10 mt-2">
        <div class="image_container">
          <router-link to="/">
            <v-img
              class="google__logo_result"
              src="https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png"
              alt="Google logo"
              contain
            />
          </router-link>
        </div>
      </div>
      <div class="search_section">
        <Search />
      </div>
    </div>
    <div class="header_wrapper">
      <div class="menu">
        <div class="menu_left">
          <div class="menu_item active">
            <v-icon color="#1a73e8" small> mdi-magnify</v-icon> All
          </div>
          <div class="menu_item">
            <v-icon class="mr-1" small>mdi-play-box-outline</v-icon>Video
          </div>
          <div class="menu_item"><v-icon small>mdi-image</v-icon> Images</div>
          <div class="menu_item">
            <v-icon small>mdi-map-marker-outline</v-icon> Maps
          </div>
          <div class="menu_item">
            <v-icon class="mr-1" small>mdi-newspaper</v-icon>News
          </div>
          <div class="menu_item">
            <v-icon small>mdi-dots-vertical</v-icon>More
          </div>
        </div>
        <div class="menu_right">
          <div class="menu_item">Settings</div>
          <div class="menu_item">Tools</div>
        </div>
      </div>
    </div>

    <div class="results">
      <div class="results_count pt-4 pb-5">
        About
        <span v-if="data">
          {{ data.searchInformation.formattedTotalResults }}
        </span>
        results (
        <span v-if="data">
          {{ data.searchInformation.formattedSearchTime }}
        </span>
        seconds)
      </div>
      <span v-if="data">
        <div
          class="actual_result mb-5"
          v-for="item in data.items"
          :key="item.cacheId"
        >
          <a class="display_link" :href="item.link">
            <div class="display_link">
              {{ item.displayLink }} <v-icon>mdi-menu-down</v-icon>
            </div>
          </a>
          <div class="data_title">
            <a :href="item.link">{{ item.title }}</a>
          </div>
          <!-- v-html is to render text data to html !important -->
          <div class="data_body" v-html="item.htmlSnippet"></div>
        </div>
      </span>
    </div>
  </div>
</template>
<style scoped></style>
<script>
import Search from "@/components/Search.vue";
// import Response from "@/responseData";
import store from "@/store";

export default {
  components: {
    Search,
  },
  data() {
    return {
      data: null,
    };
  },
  watch: {
    // watch store changes so i can update the results in realtime
    "$store.state.responseData": function() {
      console.log("Store changed");
      this.data = store.state.responseData;
    },
  },
  mounted: function() {
    this.data = store.state.responseData;
    console.log(this.data);
  },
};
</script>
<style scoped>
.image_container {
  width: 100px;
}
.search_section {
  width: 700px;
}
.header,
.menu,
.menu_right,
.menu_left {
  display: flex;
}

.menu .menu_item {
  padding: 10px;
  padding-left: 20px;
  cursor: pointer;
}

.menu .menu_right {
  padding-left: 71px;
}
.menu {
  margin-left: 190px;
  font-size: 12px;
}
.header_wrapper {
  border-bottom: 1px solid rgb(235, 235, 235);
}

/* Active */
.menu_item.active {
  border-bottom: 3px solid #1a73e8;
  color: #1a73e8;
}
.results_count {
  font-size: 12px;
  text-align: left;
  width: 400px;
  margin-left: 190px;
}
.actual_result {
  text-align: left;
  max-width: 700px;
  margin-left: 190px;
  word-wrap: break-word;
}
.data_title a {
  font-size: 20px;
  color: #1a0dab;
}
.data_title a:visited {
  color: #609;
}
.display_link,
.data_body {
  font-size: 14px;
}
.header {
  position: sticky;
  top: 0px;
  z-index: 1;
  background-color: #fff;
  padding-top: 14px;
}
.actual_result .display_link {
  text-decoration: none;
  color: inherit;
}
.results {
  margin-bottom: 13% !important;
}
</style>
