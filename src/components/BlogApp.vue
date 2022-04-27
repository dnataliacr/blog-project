<template>
  <div>
    <div class="search"></div>
    <section class="blog">
      <div class="blog-container" v-if="dataApp.length">
        <div v-for="item in dataApp" class="blog-box" :key="item.id">
          <div class="img">
            <img
              class="blog-img"
              :src="item.yoast_head_json.og_image[0].url"
              alt="no sale"
            />
          </div>
          <div class="blog-text">
            <h2 class="blog-title">{{ item.title.rendered }}</h2>
            <div v-html="item.excerpt.rendered"></div>
            <a :href="item.link"> View more</a>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
  data() {
      dataApp: [],
      tags: [],
  },
  mounted() {
    fetch("https://www.kiterocket.com/wp-json/wp/v2/tags").then((response) =>
      response.json().then((data) => {
        data.forEach((item) => {
          this.tags.push({ id: item.id, name: item.name });
        });
      })
    );
  },
  methods: {
    fetchPost() {
      let url = "";
      this.value === ""
        ? (url = "posts?_embed=1&per_page=20")
        : (url = "posts?tags=" + this.value);
      fetch("https://www.kiterocket.com/wp-json/wp/v2/" + url)
        .then((response) => response.json())
        .then((data) => {
          this.dataApp = data;
        });
    },
</script>
