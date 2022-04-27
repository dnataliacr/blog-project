<template>
  <div>
    <div class="page-container">
      <section id="blog">
        <div class="filter-container">
          <el-select
            v-model="value"
            @change="filter()"
            :placeholder="value.name ? value.name : 'Filter'"
          >
            <el-option
              v-for="item in tags"
              :key="item.id"
              :label="item.name"
              :value="item"
            >
            </el-option>
          </el-select>
        </div>
        <ul class="blog-filter">
          <li
            class="list"
            :class="value === '' ? 'blog-filter-active' : ''"
            @click="filterAll"
          >
            All
          </li>
          <span class="filter-line"> /</span>

          <li v-show="value" class="blog-filter-active">{{ value.name }}</li>
        </ul>

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
      page: 1,
      perPage: 20,
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
    filter() {
      this.currentFilter = this.value;
      this.fetchPost();
    },
    handleSizeChange() {
      this.fetchPost();
    },
    handleCurrentChange() {
      this.fetchPost();
    },
    filterAll() {
      this.value = "";
      this.fetchPost();
    },
  },
  created() {
    this.fetchPost();
    this.fetchTags();
  },
  components: {
    ElPagination,
    ElSelect,
    ElOption,
  },
};
</script>
