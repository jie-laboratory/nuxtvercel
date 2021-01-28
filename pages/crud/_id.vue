<template>
  <div class="">
    <h1>{{ post.title }}</h1>
    <p>{{ post.body }}</p>
  </div>
</template>

<script>
export default {
  async asyncData({ params, $axios }) {
    const post = await $axios.$get(`/jieBackpain/posts/${params.id}`)
    return { post }
  },
}
</script>
