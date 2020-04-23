<template>
  <div id="add-blog">
      <h2>Add a New Blog Post</h2>
      <form v-if="!submitted">
        <label>Blog Title:</label>
        <input v-model.lazy="blog.title" type="text" required />

        <label>Blog Content:</label>
        <textarea v-model.lazy="blog.content"></textarea>

        <div id="checkboxes">
          <input type="checkbox" id="cCulinary" value="culinary" v-model="blog.categories" />
          <label for="cCulinary">Culinary</label>
          
          <input type="checkbox" id="cReading" value="reading" v-model="blog.categories" />
          <label for="cReading">Reading</label>
          
          <input type="checkbox" id="cProg" value="programming" v-model="blog.categories" />
          <label for="cProg">Programming</label>
          
          <input type="checkbox" id="cGames" value="games" v-model="blog.categories" />
          <label for="cGames">Games</label>
        </div>

        <label>Author:</label>
        <select v-model="blog.author" title="Author">
          <option v-bind:value="author" v-bind:key="index" v-for="(author, index) in authors">
            {{author}}
          </option>
        </select>

        <button v-on:click.prevent="postBlog">Add Blog</button>
      </form>
      
      <div v-if="submitted">
        <h3>Thank you for submitting your post!</h3>
      </div>
      <div v-else-if="loading">
        <h3>Posting...</h3>
      </div>
      <div v-else-if="error">
        <h3>Error at submission of your post. Try again.</h3>
      </div>

      <div id="preview">
        <h3>Preview Blog</h3>
        <p>Blog title: {{blog.title}}</p>
        <p>Blog content:</p>
        <p>{{blog.content}}</p>
        <p>Blog Categories:</p>
        <ul>
          <li v-bind:key="index" v-for="(category, index) in blog.categories">
            {{category}}
          </li>
        </ul>
        <p>Blog Author: {{blog.author}}</p>
      </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: "AddBlog",
  data() {
    return {
        blog: {
          title: new String(),
          content: new String(),
          categories: new Array(),
          author: new String()
        },
        authors: [
          "Gabriel Cavalcante",
          "Github",
          "VSCode"
        ],
        submitted: false,
        loading: false,
        error: false
    }
  },
  methods: {
    postBlog() {
      this.loading=true;
      this.error = false;
      this.submitted = false;
      if(this.blog.author.length === 0)
        this.blog.author = 'anonnymous';
      axios.post('https://blog-posts-vue-project.firebaseio.com/posts.json',this.blog)
      .then(data => {
          console.log(data);
          this.loading = false;
          this.error = false;
          this.submitted = true;
      }).catch(err=>{
        console.error('There was an error: ', err);
        this.error = true;
        this.loading = false;
        this.submitted = false;
      });
    }
  },
  created() { this.$emit('changeOffset', '60px'); }
}
</script>

<style scoped>
  #add-blog {
      margin: 20px auto;
      max-width: 500px;
  }
  label {
      display: block;
      margin: 20px 0 10px;
  }
  textarea {
    resize: vertical;
    min-height: 150px;
  }
  button {
    display: block;
    width: 30%;
    margin: 10px auto -10px;
  }
  #preview {
      padding: 10px 20px;
      border: 1px dotted #ccc;
      margin: 30px 0;
  }
  h3 {
      margin-top: 10px;
  }
  #checkboxes input {
    display: inline-block;
  }
  #checkboxes label {
    display: inline-block;
    margin-right: 10px;
  }
</style>
