<template>
  <div v-theme:column="'wide'" id="show-blogs">
    <h1>第二天工作内容:所有博客</h1>
    <!-- search -->
    <input type="text" v-model="search" placeholder="search something..." />
    <div v-for="(blog,index) in filterdBlogs" :key="index" class="single-blog">
      <router-link :to="'/blog/'+blog.id">
        <h2 v-rainbow>{{blog.title | big}}</h2>
      </router-link>
      <article>{{blog.body | word}}</article>
    </div>
  </div>
</template>

<script>
export default {
  name: "ShowBlogs",
  data() {
    return {
      blogs: [],
      search: ""
    };
  },
  created() {
    this.$axios.get("/posts").then(res => {
      console.log(res.data);
      this.blogs = res.data.slice(0, 10);
    });
  },
  computed: {
    filterdBlogs() {
      return this.blogs.filter(blog => {
        return blog.title.match(this.search);
      });
    }
  },
  directives: {
    rainbow: {
      bind(el, binding, vnode) {
        el.style.color =
          "#" +
          Math.random()
            .toString(16) // 16进制
            .slice(2, 8);
      }
    },
    theme: {
      bind(el, binding, vnode) {
        if (binding.value == "wide") {
          el.style.maxWidth = "1260px";
        } else if (binding.value == "narrow") {
          el.style.maxWidth = "560px";
        }

        if (binding.arg == "column") {
          el.style.background = "blue";
          el.style.padding = "20px";
        }
      }
    }
  },
  filters: {
    /*
    "to-uppercase": function(value) {
      return value.toUpperCase();
      */
    big(value) {
      return value.toUpperCase();
    },
    word(value) {
      return value.slice(0, 100) + "...";
    }
  }
};
</script>

<style scoped>
#show-blogs {
  max-width: 800px;
  margin: 0 auto;
}

.single-blog {
  padding: 20px;
  margin: 20px 0;
  box-sizing: border-box;
  background: #eeeeee;
}

input[type="text"] {
  padding: 8px;
  width: 100%;
  box-sizing: border-box;
}
</style>
