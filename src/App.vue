<template>
  <NavBar />
  <router-view :key="$route.fullPath" />
</template>

<script>
import NavBar from './components/NavBar';

export default {
  name: "App",
  components: {
    NavBar
  }
}

</script>

<style scoped>
</style>
