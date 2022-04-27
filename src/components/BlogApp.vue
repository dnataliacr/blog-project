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
