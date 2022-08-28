<template>
  <div class="wrapper">
    <div class="container">
      <form-search v-model="searchQuery" />
      <h2 v-if="!searchedPosts.length">Постов такого автора нет...</h2>
      <div v-else>
        <post-list :posts="searchedPosts" />
      </div>
    </div>
  </div>
</template>

<script>
import FormSearch from '@/components/formSearch.vue'
import PostList from '@/components/postList.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    FormSearch,
    PostList,
  },
  data() {
    return {
      searchQuery: '',
      users: {},
      posts: [],
    }
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await axios.get('http://jsonplaceholder.typicode.com/users')
        this.users = response.data.reduce((acc, user) => {
          acc[user.id] = user
          return acc
        }, {})
        this.fetchPosts()
      } catch (e) {
        alert('Ошибка в получении авторов')
      }
    },
    async fetchPosts() {
      try {
        const response = await axios.get('http://jsonplaceholder.typicode.com/posts')
        this.posts = response.data
      } catch (e) {
        alert('Ошибка в получении постов')
      }
    },
  },
  mounted() {
    this.fetchUsers()
  },
  computed: {
    postsWithAuthor() {
      return this.posts.map((post) => {
        return { ...post, author: this.users[post.userId].name }
      })
    },
    searchedPosts() {
      return this.postsWithAuthor.filter((post) => post.author.toLowerCase().includes(this.searchQuery.toLowerCase()))
    },
  },
}
</script>
