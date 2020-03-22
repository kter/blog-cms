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
            <img class="featured-image" v-else :src="'https://picsum.photos/seed/' + Date.parse(post.published) + '/250/250.jpg'" alt="">
          </figure>
          <div class="article-text">
            <h2>{{ post.title }}</h2>
            <p>{{ post.short_summary }}</p>
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

const striptags = require('striptags')
const SUMMARY_LIMIT = 90

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
        const rowSize = res.data.data.length
        for (let i = 0; i < rowSize; i++) {
          res.data.data[i].short_summary = this.shortSummary(res.data.data[i].summary)
        }
        this.posts = res.data.data
      })
    },
    shortSummary (summary) {
      return striptags(summary).slice(0, SUMMARY_LIMIT) + '...'
    }
  },
  created () {
    this.getPosts()
  }
}
</script>
