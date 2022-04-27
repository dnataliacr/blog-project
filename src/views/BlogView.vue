<template>
  <div>
    <div class="page-container">
      <el-pagination
        v-model:currentPage="page"
        v-model:page-size="perPage"
        layout="prev, pager, next, sizes"
        :page-sizes="[5, 10, 20, 50]"
        :total="1000"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>
  </div>
</template>

<script>
import { ElPagination, ElSelect, ElOption } from "element-plus";
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
    handleSizeChange() {
      this.fetchPost();
    },
    handleCurrentChange() {
      this.fetchPost();
    },
  },
  created() {
    this.fetchPost();
    this.fetchTags();
  },
  components: {
    ElPagination,
  },
};
</script>
