<template>
  <v-app>
     <Nav />
     <v-footer>
       <Footer />
     </v-footer>
  </v-app>
 
</template>

<script>
import Nav from './components/Nav.vue'
import Footer from './components/Footer.vue'


export default {
  name: 'App',
  components: {
      Nav,
      Footer
  },
};
</script>


