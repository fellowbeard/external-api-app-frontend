<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      posts: [],
      articles: [],
    };
  },
  created: function () {
    this.postsIndex();
    this.newsIndex();
    var code = this.$route.query.code;
    console.log(code);
    if (code) {
      axios.get("/auth/github/callback?code=" + code).then((response) => {
        localStorage.setItem("github_access_token", response.data.access_token);
        console.log(response.data.access_token);
        this.$router.push("/about");
      });
    }
  },
  methods: {
    postsIndex: function () {
      axios.get("https://jsonplaceholder.typicode.com/posts").then((response) => {
        console.log(response.data);
        this.posts = response.data;
      });
    },
    newsIndex: function () {
      axios.get("/news_api").then((response) => {
        console.log(response.data);
        this.articles = response.data.articles;
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <a href="https://github.com/login/oauth/authorize?client_id=7b0f38f873ca98fcece8">Get in to Github</a>
    <div v-for="post in posts" v-bind:key="post.id">
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </div>
  </div>
</template>

<style></style>
