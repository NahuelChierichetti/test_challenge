<template>
    
    <v-app>
        <v-app-bar
        app
        class="barra"
        dark
        >
        <div class="d-flex align-center">
            <h1>logo</h1>
        </div>

        <v-spacer></v-spacer>

        <v-btn text>
            <span class="mr-2"><v-icon x-large>mdi-account-circle</v-icon></span>
        </v-btn>
        </v-app-bar>

        <v-main>
            <router-view/>
        </v-main>
    </v-app>

</template>

<style>
  .barra{
    background: rgb(108,64,156);
    background: linear-gradient(90deg, rgba(108,64,156,1) 5%, rgba(18,169,200,1) 100%);
  }
</style>