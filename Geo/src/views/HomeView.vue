<script setup>
import TheWelcome from '../components/Posts.vue'
import { ref, computed } from 'vue'
import { useStore } from '../stores/mainStore'

let store = useStore();
store.getPosts();
store.getComments();
let posts = computed(() => {
  let p = store.postsArray;
  p.forEach(item => {
    item.comments = store.commentsArray.filter(i => i.postId === item.id)
  })
  return p
})
</script>
<template>
  <main>
    <div v-if="posts.length > 0">
      <TheWelcome :posts="posts"/>
    </div>
  </main>
</template>
