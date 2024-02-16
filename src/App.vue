<template>
  <div id="app">
    <PostFetcher />
  </div>
</template>

<script>
import PostFetcher from './components/PostFetcher.vue';

export default {
  components: {
    PostFetcher,
  },
};
</script>

<style>
#app {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  margin: 0;
  background-color: #87ceeb;
}

body {
  margin: 0;
  padding: 0;
}
</style>