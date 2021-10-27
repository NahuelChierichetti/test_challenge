<template>
  <v-container 
  fluid>
    <Tabla />
  </v-container>
</template>

<script>
  import Tabla from '../components/Tabla'

  export default {
    name: 'Home',

    components: {
      Tabla
    },
  }
</script>
