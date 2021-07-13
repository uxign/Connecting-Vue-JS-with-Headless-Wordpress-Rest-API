<template>
    <div>
        <h2>posts list</h2>
    </div>
</template>

<script>


export default ({
name : "posts",
data: function(){
    return{
        posts: []
    }
},
  created: function () {
    //get api posts list
    this.$http.get("wp/v2/posts").then((response) => {
      
      for(let posts in response.data){
        this.posts.push(response.data[posts]);
      }
    }, error => {
      alert(error);
    });

  },
})
</script>


