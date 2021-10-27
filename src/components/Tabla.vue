<template>
  <v-data-table
    :headers="headers"
    :items="usuarios"
    class="elevation-1"
    hide-default-footer
  >
    <template
      v-slot:top
    >
      <v-toolbar
        flat
      >
        <v-toolbar-title>Usuarios</v-toolbar-title>

        <v-spacer></v-spacer>
        <v-dialog 
          v-model="dialog" 
          max-width="800px"
        >
          <v-card>
            <v-card-title
              class="title"
            >
              <span 
                class="text-h5" 
                style="color: white"
              >
                Editar Usuario
              </span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12">
                    <v-text-field
                      v-model="editedItem.username"
                      label="Usuario"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.email"
                      label="Email"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.phone"
                      label="Teléfono"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.address.street"
                      label="Calle"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.address.suite"
                      label="Departamento"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.address.city"
                      label="Ciudad"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.address.zipcode"
                      label="Código Postal"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.website"
                      label="Website"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      v-model="editedItem.company.name"
                      label="Empresa"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      v-model="editedItem.company.bs"
                      label="Descripción"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn 
                color="blue darken-1" 
                text @click="close"
              >
                Cancelar
              </v-btn>
              <v-btn 
                color="blue darken-1" 
                text @click="save"
              > 
                Guardar 
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <v-dialog 
          v-model="dialogDelete" 
          max-width="400px"
        >
          <v-card>
            <v-card-title
              class="text-h5"
            >
              Desea eliminar este usuario?
            </v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn 
                color="blue darken-1" 
                text @click="closeDelete"
              >
                Cancelar
              </v-btn>
              <v-btn 
                color="red darken-1" 
                text @click="confirmarBorrado"
              >
                Confirmar
              </v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:[`item.options`]="{ item }">
      <v-btn 
        icon 
        @click.stop="dialogDelete = true" 
        @click="deleteItem(item)"
      >
        <v-icon 
          small 
          class="mr-2"
        > 
          mdi-delete 
        </v-icon>
      </v-btn>
      <v-btn 
        icon 
        @click="editItem(item)" 
        target="_blank"
      >
        <v-icon 
          small 
          class="mr-1" 
          @click="altaPost(item)"
        > 
          mdi-pencil 
        </v-icon>
      </v-btn>

      <v-btn
        icon
        :to="{ name: 'AltaPosteos', params: { id: item.id } }"
        target="_blank"
      >
        <v-icon 
          small
        > 
          mdi-play 
        </v-icon>
      </v-btn>
    </template>
  </v-data-table>
</template>

<script>
let url = "https://jsonplaceholder.typicode.com/users/";

import axios from "axios";

export default {
  mounted() {
    this.listarUsuarios();
  },
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: "Nombre",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Usuario", value: "username" },
      { text: "Email", value: "email" },
      { text: "Teléfono", value: "phone" },
      { text: "Website", value: "website" },
      { text: "Nombre empresa", value: "company.name" },
      { text: "Opciones", value: "options", sortable: false },
    ],
    usuarios: [],
    editedIndex: -1,
    editedItem: {
      id: "",
      username: "",
      email: "",
      phone: "",
      address: "",
      website: "",
      company: "",
    },
    defaultItem: {
      username: "",
      email: "",
      phone: "",
      address: "",
      website: "",
      company: "",
    },
  }),

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  methods: {
    listarUsuarios() {
      axios
        .get(url)
        .then((res) => {
          this.usuarios = res.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    deleteItem(item) {
      this.editedIndex = this.usuarios.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    confirmarBorrado() {
      this.usuarios.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    /*confirmarBorrado(){
        console.log(this.editedItem.id)
        axios.delete(url+this.editedItem.id)
        .then(()=>{
          //console.log(url+this.editedItem.id) --> https://jsonplaceholder.typicode.com/users/1
          this.listarUsuarios()
          this.dialogDelete = false
        })
      },*/

    altaPost(item) {
      this.editedIndex = this.usuarios.indexOf(item);
      this.editedItem = Object.assign(this.usuarios[this.editedIndex]);
    },

    editItem(item) {
      this.editedIndex = this.usuarios.indexOf(item); 
      this.editedItem = Object.assign({}, item); 
      this.dialog = true;
    },

    save() {
      let router = this.$router;
      axios.get(url + this.editedItem.id).then(() => {
        if (this.editedIndex > -1) {
          let params = Object.assign(
            this.usuarios[this.editedIndex],
            this.editedItem
          );
          axios.put(url + this.editedItem.id, params);
        } else {
          router.push("/");
        }
        this.close();
      });
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },
  },
};
</script>

<style scoped>
.title {
  background: rgb(108, 64, 156);
  background: linear-gradient(
    90deg,
    rgba(108, 64, 156, 1) 5%,
    rgba(18, 169, 200, 1) 100%
  );
}
</style>