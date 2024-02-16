<template>
  <div>
    <!-- Container for the fetched text -->
    <div id="textContainer"
         ref="textContainer"
         :style="{ maxHeight: containerHeight }"
         @click="toggleFetching"
         @mouseover="pauseFetching"
         @mouseout="resumeFetching">
      {{ lastFetchedText }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // State variables
      lastFetchedText: '',
      containerHeight: 'auto',
      isFetchingPaused: false,
      intervalId: null,
    };
  },
  methods: {
    async fetchRandomPost() {
      try {
        const postId = Math.floor(Math.random() * 100) + 1;
        const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`);
        if (!response.ok) {
          throw new Error('Failed to fetch data');
        }
        const data = await response.json();
        // Update the text and container height
        this.lastFetchedText = data.body;
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
    // Update the container height based on the text content
    updateContainerHeight() {
      const height = this.$refs.textContainer.scrollHeight; // Get the current height
      this.containerHeight = `${height}px`; // Set the container height
    },
    // Toggle fetching state on container click
    toggleFetching() {
      this.isFetchingPaused = !this.isFetchingPaused;
      if (!this.isFetchingPaused) {
        this.fetchRandomPost();
      }
    },
    // Pause fetching on container mouseover
    pauseFetching() {
      if (!this.isFetchingPaused) {
        clearInterval(this.intervalId);
      }
    },
    // Resume fetching on container mouseout
    resumeFetching() {
      if (!this.isFetchingPaused) {
        this.intervalId = setInterval(() => {
          this.fetchRandomPost();
        }, 2000);
      }
    },
  },
  watch: {
    lastFetchedText() {
      this.$nextTick(() => {
        this.updateContainerHeight();
      });
    },
  },
  mounted() {
    // Fetch a random post from the API
    this.fetchRandomPost();
    // Set up interval for periodic fetching
    this.intervalId = setInterval(() => {
      if (!this.isFetchingPaused) {
        this.fetchRandomPost();
      }
    }, 2000);
  },
};
</script>

<style scoped>
#textContainer {
  max-width: 20ch;
  min-width: 20ch;
  margin: auto;
  padding: 2ch;
  font-size: 2rem;
  background-color: #ffffff;
  border: 2px solid #000000;
  cursor: pointer;
  text-align: center;
  overflow: hidden;
  transition: max-height 0.5s ease-in-out; /* Smooth transition for max-height */
}
</style>