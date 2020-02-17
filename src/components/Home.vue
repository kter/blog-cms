<template>
  <div id="home">
    <h1>{{ page_title }}</h1>
    <!-- Create v-for and apply a key for Vue. Example is using a combination of the slug and index -->
    <div class="article-wrapper" v-for="(post,index) in posts" :key="post.slug + '_' + index">
      <router-link :to="'/' + post.slug">
        <article class="media article">
          <figure class="article-image">
            <!-- Bind results using a ':' -->
            <!-- Use a v-if/else if their is a featured_image -->
            <img class="featured-image" v-if="post.featured_image" :src="post.featured_image" alt="">
            <img class="featured-image" v-else :src="'https://i.picsum.photos/id/' + getRand(Date.parse(post.published)) + '/250/250.jpg'" alt="">
          </figure>
          <div class="article-text">
            <h2>{{ post.title }}</h2>
            <p>{{ post.summary }}</p>
          </div>
        </article>
      </router-link>
    </div>
  </div>
</template>

<style>
  #home {
    margin: 50px;
    text-align: initial;
  }
  .article-wrapper {
    margin: 25px;
  }
  .article {
    display: table;
    width: 100%;
    table-layout: fixed;
  }

  .article-image {
    display: table-cell;
    width: 250px;
    padding-right: 25px;
  }

  .article-text {
    display: table-cell;
    vertical-align: middle;
  }

  .featured-image {
    width: 250px;
  }

</style>

<script>
import { butter } from '@/buttercms'
import { Random } from '@/random'
export default {
  name: 'home',
  data () {
    return {
      page_title: 'Blog',
      posts: []
    }
  },
  methods: {
    getPosts () {
      butter.post.list({
        page: 1,
        page_size: 10
      }).then((res) => {
        // console.log(res.data)
        this.posts = res.data.data
      })
    },
    getRand (seed) {
      let rand = new Random(seed)
      return rand.nextInt(0, 999)
    }
  },
  created () {
    this.getPosts()
  }
}
</script>
