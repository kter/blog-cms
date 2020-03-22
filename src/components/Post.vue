<template>
  <div id="post">
    <h1>{{ post.data.title }}</h1>
    <div id="post-info">
      Published at: {{ post.data.published }}
    </div>
    <hr>
    <div v-html="post.data.body"></div>
    <hr>

    <table>
      <tr>
        <td>
          Next Post:
        </td>
        <td>
          <router-link v-if="post.meta.previous_post" :to="post.meta.previous_post.slug" class="button">
            {{ post.meta.previous_post.title }}
          </router-link>
        </td>
      </tr>
      <tr>
        <td>
          Previous Post:
        </td>
        <td>
          <router-link v-if="post.meta.next_post" :to="post.meta.next_post.slug" class="button">
            {{ post.meta.next_post.title }}
          </router-link>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import { butter } from '@/buttercms'
export default {
  name: 'post',
  data () {
    return {
      post: {}
    }
  },
  methods: {
    getPost () {
      butter.post.retrieve(this.$route.params.slug)
        .then((res) => {
          // console.log(res.data)
          this.post = res.data
        }).catch((res) => {
          console.log(res)
        })
    }
  },
  created () {
    this.getPost()
  },
  watch: {
    $route (to, from) {
      this.getPost()
    }
  }
}
</script>
