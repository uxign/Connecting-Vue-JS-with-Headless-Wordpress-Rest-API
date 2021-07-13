<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h4>MAin new page api</h4>

    <!-- <div v-for="cat in categories" :key="cat">
      <a :href="cat.link">{{cat.name}}</a>
    </div>


    <div v-for="proj in project" :key="proj">
      <img :src="proj.better_featured_image.media_details.sizes.medium.source_url" alt="">
      <h1><a :href="proj.link">{{proj.title.rendered}}</a></h1>     
    </div> -->

    <div class="pos" v-for="post in posts" :key="post">
      <h1><a :href="post.link">{{ post.title.rendered }}</a></h1>
    </div>
    
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
    data: function(){
    return{
      
      // project: [],
      // categories:[],
      postsUrl : 'wp/v2/posts',
      posts: [],
      postsData:{
        per_page: 10,
        page : 1,
      } 
    }
  },
  methods:{

    getPosts(){

    }
  },
  created: function () {
    //get api project list
    this.$http.get("wp/v2/project").then((response) => {
      
      for(let project in response.data){
        this.project.push(response.data[project]);
      }
    }, error => {
      alert(error);
    });
    //get api categories
    this.$http.get("wp/v2/categories").then((response) => {
      
      for(let categories in response.data){
        this.categories.push(response.data[categories]);
      }
    }, error => {
      alert(error);
    });
        //get api posts list
    this.$http.get("wp/v2/posts").then((response) => {
      
      for(let posts in response.data){
        this.posts.push(response.data[posts]);
      }
    }, error => {
      alert(error);
    });
  },
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
