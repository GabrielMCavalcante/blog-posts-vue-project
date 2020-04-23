<template>
  <div id="show-blogs" v-theme:column="'wide'">
    <h1>All Blog Articles</h1>
    <input type="text" v-model="search" placeholder="Search post..." />
    <div v-bind:key="index" v-for="(blog, index) in filteredBlogs" class="single-blog">
        <button v-bind:id="'del-btn-'+blog.id" v-on:click="deletePost(blog.id)">Delete post</button>
        <router-link v-bind:to="'/blog/'+blog.id">
            <h2 style="width:89%;margin-top:40px;" v-rainbow>{{blog.title | toUppercase}}</h2>
            <article>{{blog.content | snippet}}</article>
        </router-link>
    </div>
    <div v-if="blogs.length === 0">
        <h3>No posts yet - submit one now!</h3>
    </div>
    <div v-else-if="filteredBlogs.length === 0">
      <h3>No post found...</h3>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import SearchMixin from "../mixins/SearchMixin.js";
export default {
  name: "ShowBlogs",
  data() {
    return {
      blogs: new Array(),
      search: new String()
    };
  },
  mixins: [SearchMixin],
  beforeCreate() {
    axios.get("https://blog-posts-vue-project.firebaseio.com/posts.json")
      .then(res => {
        this.blogs = new Array();
        for (const key in res.data) {
          res.data[key].id = key;
          this.blogs.push(res.data[key]);
        }
      }).catch(err => console.error("There has been an error: ", err));
  },
  created() {
    this.$emit("changeOffset", "25px");
  },
  filters: {
    toUppercase(value) {
      return value.toUpperCase();
    },
    snippet(value) {
      if (value.length > 100) return value.slice(0, 100) + "...";
      else return value;
    }
  },
  directives: {
    rainbow: {
      bind(el) {
        el.style.color =
          "#" +
          Math.random()
            .toString(10)
            .slice(2, 8);
      }
    },
    theme: {
      bind(el, binding) {
        if (binding.value === "wide") el.style.maxWidth = "1200px";
        else if (binding.value === "narrow") el.style.maxWidth = "540px";

        if (binding.arg === "column") {
          el.style.padding = "20px";
          el.style.boxShadow = "1px 1px 4px rgba(0,0,0,.4)";
        }
      }
    }
  },
  methods: {
    deletePost(id) {
      axios.delete("https://blog-posts-vue-project.firebaseio.com/posts/" + id + ".json")
        .then(() => {
          document.getElementById('del-btn-'+id).innerHTML = 'Deleting...';
          // Updating posts list
          axios.get("https://blog-posts-vue-project.firebaseio.com/posts.json")
            .then(res => {
              this.blogs = new Array();
              for (const key in res.data) {
                res.data[key].id = key;
                this.blogs.push(res.data[key]);
              }
            }).catch(err => console.error("There has been an error: ", err));

        }).catch(err => console.error("There has been an error: " + err));
    }
  }
};
</script>

<style scoped>
#show-blogs {
  background: #222425;
  max-width: 800px;
  margin: 0px auto;
  
}
#show-blogs h1 {
  text-align: center;
}
.single-blog {
  padding: 20px;
  margin: 20px 0;
  box-sizing: border-box;
  background-color: rgb(29, 31, 32);
  text-align: justify;
}
.single-blog button {
  float: right;
}
.single-blog a {
  text-decoration: none;
  color: inherit;
  position: relative;
} 
</style>