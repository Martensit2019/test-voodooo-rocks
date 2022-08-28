<template>
  <div class="posts__list">
    <div v-for="post in posts" class="post__item" :key="post.id">
      <div class="post__title">{{ post.title[0].toUpperCase() + post.title.slice(1) }}</div>
      <div class="post__body">{{ post.body[0].toUpperCase() + post.body.slice(1) }}</div>
      <div class="post__uauthor">{{ post.author }}</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    posts: {
      type: Array,
      default: [],
    },
  },
}
</script>
