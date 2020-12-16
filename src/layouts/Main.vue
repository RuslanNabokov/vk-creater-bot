<template>
  <div>
     <!-- <Loader v-if="loading" /> -->
    <!-- <div class="app-main-layout" v-else> -->
      
      <!-- <Navbar @click="isOpen = !isOpen" /> -->

      <!-- <Sidebar v-model="isOpen" /> -->
      
      <main class="app-content" :class="{full: !isOpen}">
        <div class="app-page">
          <Navbar />
          <Sidebar />
          <ShimeRedactor />
          
          <router-view />
        </div>
      </main>

      <div class="fixed-action-btn">
        <!-- <router-link class="btn-floating btn-large blue" to="/record"> -->
          <i class="large material-icons">add</i>
        <!-- </router-link> -->
      </div>
    </div>
</template>

<script>

import Navbar from '@/components/main/Navbar'
import Sidebar from '@/components/main/Sidebar'
import ShimeRedactor from '@/components/ShimeRedactor'

export default {
  components: { Navbar, Sidebar,ShimeRedactor},
    data:() => ({
        isOpen:true,
    }),

}
</script>