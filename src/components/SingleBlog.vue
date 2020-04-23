<template>
    <div id="single-blog" v-if="loaded">
        <h1>{{blog.title}}</h1>
        <article>{{blog.content}}</article>
        <p>Author: {{blog.author}}</p>
        <p>Categories:</p>
        <ul>
            <li v-bind:key="index" v-for="(category, index) in blog.categories">
                {{category}}
            </li>
        </ul>
    </div> 
</template>

<script>
import axios from 'axios';
export default {
    name: "SingleBlog",
    data() {
        return {
            id: this.$route.params.id,
            blog: new Object(),
            loaded: false
        }
    },
    created() {
        axios.get('https://blog-posts-vue-project.firebaseio.com/posts/'+this.id+'.json')
            .then(res=>{
                this.blog = res.data;
                this.loaded = true;
            }).catch(err=>console.error('There has been an error: ', err));
        this.$emit('changeOffset', '55px');
    }
}
</script>

<style scoped>
    #single-blog {
        max-width: 960px;
        margin: 0 auto;
    }
    article {
        text-align: justify;
    }
</style>