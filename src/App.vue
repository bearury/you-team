<template>
  <HomeView />
</template>

<script>
import HomeView from '@/views/AuthView.vue';

export default {
  components: {
    HomeView,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #272b34;
  color: aliceblue;
}
</style>
