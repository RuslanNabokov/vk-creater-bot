<template>
  <div id="app">
    <component :is='layout'>

    </component>
  </div>
</template>
<script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
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
