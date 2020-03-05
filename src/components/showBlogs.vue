<template>
  <div v-theme:column="'narrow'" id="show-blogs">
    <h1>All Blog Articles</h1>
    <input type="text" v-model="search" placeholder="search blogs"/>
    <div v-for="blog in filteredBlogs" class="single-blog">
      <router-link v-bind:to="'/blog/' + blog.id"><h2 v-rainbow>{{blog.title | toUppercase}}</h2></router-link>
      <article>{{blog.content | snippet}}</article>

      <!-- <router-link v-bind:to="'/blog/' + blog.id"><h2 v-rainbow>{{blog.title | toUppercase}}</h2></router-link>
      <article>{{blog.body | snippet}}</article> -->
    </div>
  </div>
</template>

<script>
import searchMixin from '../mixins/searchMixin';

export default {
  data () {
    return {
      blogs: [],
      search: ''
    }
  },
  methods: {
    
  },
  
  created(){
    this.$http.get('https://vue-test1-8c174.firebaseio.com/posts.json').then(function(data){
      return data.json();
    }).then(function(data){
      var blogsArray =[];
      for (let key in data){
        data[key].id = key;
        blogsArray.push(data[key]);
      }
      console.log(blogsArray);
      this.blogs = blogsArray;
    })
  },

  // JsonPlaceholder - fake resource
  // created(){
  //   this.$http.get('https://jsonplaceholder.typicode.com/posts').then(function(data){
  //     // console.log(data);
  //     this.blogs = data.body.slice(0,10);
  //   })
  // },
  
  filters: {
    // 'to-uppercase':function(value){
    toUppercase(value){
      return value.toUpperCase();
    },
    snippet(value){
      return value.slice(0,100)+'...';
    }
  },
  directives: {
    'rainbow': {
      bind(el, binding, vnode){
        el.style.color = "#"+ Math.random().toString().slice(2,8);
      }
    }
  },
  mixins:[searchMixin]
}
</script>

<style>
#show-blogs{
  max-width: 800px;
  margin: 0 auto;
}
#show-blogs input[type=text]{
  width: 96%;
  padding: 10px;
}
.single-blog{
  padding: 20px;
  margin: 20px 0;
  box-sizing: border-box;
  background: #eee;
}
a {
  text-decoration: none;
}
</style>
