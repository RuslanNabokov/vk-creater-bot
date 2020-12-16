<template>
  <div id="app">
    <component :is='layout'>

    </component>
  </div>
</template>

<script>
import Main  from  '@/layouts/Main'
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

<style type="sass">
@import "~materialize-css/dist/css/materialize.min.css";

/* @import "assets/my.css"; */
</style>
