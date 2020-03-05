<template>
  <div v-theme:column="'narrow'" id="show-blogs">
    <h1>List Blog Titles</h1>
    <input type="text" v-model="search" placeholder="search blogs"/>
    <div v-for="blog in filteredBlogs" class="single-blog">
      <h2 v-rainbow>{{blog.title | toUppercase}}</h2>
    </div>
  </div>
</template>

<script>
import searchMixin from '../mixins/searchMixin';

export default {
  mixins: [searchMixin],
  data () {
    return {
      blogs: [],
      search: ''
    }
  },
  methods: {
    
  },
  created(){
    this.$http.get('https://jsonplaceholder.typicode.com/posts').then(function(data){
      // console.log(data);
      this.blogs = data.body.slice(0,10);
    })
  },
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
  }
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

</style>
