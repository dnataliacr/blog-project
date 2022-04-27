<template>
  <div>
    <div class="page-container">
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      data: [],
      value: "",
      tags: [],
      currentFilter: "",
  methods: {
    fetchPost() {
      let url;
      this.value === ""
        ? (url = `posts?_embed=1&per_page=${this.perPage}&page=${this.page}`)
        : (url = "posts?tags=" + this.value.id);
      fetch("https://www.kiterocket.com/wp-json/wp/v2/" + url)
        .then((response) => response.json())
        .then((data) => {
          this.data = data;
        });
    },
    fetchTags() {
      fetch("https://www.kiterocket.com/wp-json/wp/v2/tags").then((response) =>
        response.json().then((data) => {
          data.forEach((item) => {
            this.tags.push({ id: item.id, name: item.name });
          });
        })
      );
    },
  },
  created() {
    this.fetchPost();
    this.fetchTags();
  },
};
</script>
