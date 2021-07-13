<template>
  <div class="hello">
    <div class="container">
      <div class="row">
        <div class="col-lg-4 mb-30" v-for="post in posts" :key="post">
          <img class="w-100" :src="post.better_featured_image.source_url" :key="post" />
          <div class="content">
            <h4>
              <a :href="post.link" :key="post">{{ post.title.rendered }}</a>
            </h4>
            <p v-text="post.excerpt.rendered"></p>
          </div>
        </div>
      </div>
    </div>
    <div class="container text-center">
      <pagination
        :pagination="pagination"
        @prev="
          --postsData.page;
          getPosts();
        "
        @next="
          postsData.page++;
          getPosts();
        "
      >
      </pagination>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, axios);
import pagination from "./pagination.vue";
export default {
  name: "HelloWorld",
  components: {
    pagination: pagination,
  },
  mounted() {
    this.getPosts();
  },
  data: function () {
    return {
      postsUrl: "http://headless.test/admin/wp-json/wp/v2/posts",
      posts: [],
      postsData: {
        per_page: 6,
        page: 1,
      },
      pagination: {
        prevPage: "",
        nextPage: "",
        totalPages: "",
        from: "",
        to: "",
        total: "",
      },
    };
  },
  methods: {
    getPosts() {
      axios
        .get(this.postsUrl, { params: this.postsData })
        .then((response) => {
          //console.log(response);
          this.posts = response.data;
          this.configPagination(response.headers);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    configPagination(headers) {
      this.pagination.total = +headers["x-wp-total"];
      this.pagination.totalPages = +headers["x-wp-totalpages"];
      this.pagination.from = this.pagination.total
        ? (this.postsData.page - 1) * this.postsData.per_page + 1
        : " ";
      this.pagination.to =
        this.postsData.page * this.postsData.per_page > this.pagination.total
          ? this.pagination.total
          : this.postsData.page * this.postsData.per_page;
      this.pagination.prevPage =
        this.postsData.page > 1 ? this.postsData.page : "";
      this.pagination.nextPage =
        this.postsData.page < this.pagination.totalPages
          ? this.postsData.page + 1
          : "";
    },
  },
};
</script>
<style scoped>
h4 a {
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  text-decoration: none;
  color: #000;
}

p {
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
.content {
      padding: 10px;
    text-align: left;
}
.mb-30 {
  margin-bottom: 30px;
}
</style>