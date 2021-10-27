<template>
    <v-container>
        <v-layout
        class="mt-3">
            <v-btn 
            icon
            :to="{name: 'Home'}"
            >
                <v-icon
                medium
                >
                    mdi-chevron-left
                </v-icon>
                Volver
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn 
                rounded
                color="teal accent-4"
                dark
                @click="altaPost(posteosHabilitados)"
            >
            Habilitar
            </v-btn>
            <v-btn
                rounded
                @click="alta == false"
            >
            Deshabilitar
            </v-btn>
        </v-layout>
        <v-container grid-list-lg>
            <v-layout row wrap>
            <v-flex xs12 sm4 md3
                v-for="posteo in posteos"
                :key="posteo.id"
                v-show="id == posteo.userId"
                class="my-5"
            >
                <v-card 
                    style="min-height: 100%"
                >
                    <v-card-actions 
                        class="pt-0" 
                    >
                    <v-btn 
                        rounded
                        small
                        dark
                        color="teal accent-4"
                        v-if="posteo.id == posteosHabilitados ? true : false"
                    >
                    Habilitado
                    </v-btn>
                    <v-btn
                        rounded
                        small
                        v-else
                    >
                    Deshabilitado
                    </v-btn>
                    <v-spacer></v-spacer>
                    <input 
                        type="checkbox" 
                        name="alta"
                        :value="posteo.id"
                        v-model="posteosHabilitados"
                    >
                    </v-card-actions>
                    <v-card-text class="pb-0">
                    <p class="text-h4 text--primary">
                        {{posteo.title}}
                    </p>
                    <p>Posteo de Usuario: {{posteo.userId}}</p>
                    <p>Posteo id: {{posteo.id}}</p>
                    <p>{{posteo.body}}</p>
                    </v-card-text>
                    
                </v-card>
            </v-flex>
        </v-layout>
        </v-container>        
    </v-container>
</template>

<script>
    let url = 'https://jsonplaceholder.typicode.com/users/'
    let urlPosteos = 'https://jsonplaceholder.typicode.com/posts'
    import axios from 'axios'

export default {
    name: 'AltaPosteos',
    mounted(){
        this.id = this.$route.params.id
        axios.get(url+this.id)
        .then(res =>{
            this.usuario = res.data[0]
        })
        this.obtenerPosteos()
    },
    data:() => ({
        posteos: [],
        id: '',
        usuario: {
            name: '',
        },
        posteosHabilitados: [],
        alta: false,
    }),
    methods:{
        obtenerPosteos(){
            axios.get(urlPosteos)
                .then(res =>{
                    this.posteos = res.data
                })
                .catch((error)=>{
                    console.log(error)
                })
        },
        altaPost(posteosHabilitados){
            posteosHabilitados.forEach(() => {
                this.alta = true
            })     
        }
    }
}

</script>

<style scoped>

</style>