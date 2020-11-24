<template>
  <div id="app">
    <component :is='layout'>

    </component>
  </div>
</template>

<script>

const Main = {template:"<h1> <router-view /> </h1>"}
export default {
  name: 'App',
  computed: {
    layout(){

      return this.$route.meta.layout
    }
  },
  components: {
  Main
  }
}
</script>

<style>
</style>
