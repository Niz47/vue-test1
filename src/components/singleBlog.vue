<template>
    <div id="single-blog">
        <h1>{{blog.title}}</h1>
        <article>{{blog.content}}</article>

        <!-- <article>{{blog.body}}</article> -->

        <p>Author: {{blog.author}}</p>
        <ul>
            <li v-for="category in blog.categories">{{category}}</li>
        </ul>
    </div>
</template>

<script>
export default {
    data(){
        return{
            id: this.$route.params.id,
            blog: {}
        }
    },
    created(){
        // this.$http.get("https://jsonplaceholder.typicode.com/posts/"+this.id).then(function(data){
        //     console.log(data);
        //     this.blog = data.body;
        // });
        this.$http.get("https://vue-test1-8c174.firebaseio.com/posts/"+this.id+".json").then(function(data){
            console.log(data);
            return data.json();
        }).then(function(data){
            this.blog = data;
        });
    }
}
</script>

<style scoped>
#single-blog{
    padding: 10px 80px;
}
</style>