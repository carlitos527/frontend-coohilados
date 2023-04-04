<template>
  <v-container
    ><br />

    <v-row>
      <v-col colored-border type="warning">
        <v-card class="mx-auto" max-width="10000" tile>
          <template slot="progress">
            <v-progress-linear
              color="deep-purple"
              height="10"
              indeterminate
            ></v-progress-linear>
          </template>
          <v-row>
            <v-col class="yellow accent-1">
              <v-row align="end" class="fill-height">
                <v-col align-self="start" class="pa-0" cols="12">
                  <v-avatar class="profile" color="grey" size="164" tile>
                    <v-img
                      src="https://cdn.vuetifyjs.com/images/profiles/marcus.jpg"
                    ></v-img>
                  </v-avatar>

                  <v-card-text>
                    <v-row align="center" class="mx-0">
                      <v-col cols="12" sm="6" md="4">
                        <p label="Estado">
                          {{ `Área: ${$store.state.usuario.area}` }} <br />
                          {{ `Rol: ${$store.state.usuario.rol}` }} <br />
                          {{ `Estado: ${$store.state.usuario.estado}` }}
                        </p>
                      </v-col>
                      <v-card-title>{{  `${$store.state.usuario.nombre}`}}</v-card-title>
                    </v-row>
                  </v-card-text>

                  

                  <v-divider class="mx-4"></v-divider>

                  <!-- editar usuario -->
                  <v-card-text>
                    <template>
                      <v-container>
                        <v-row>
                          <v-col cols="12" sm="6" md="6">
                            <v-text-field
                              v-model="detalleUsuario.documento"
                              
                              label="Documento"
                            ></v-text-field>
                          </v-col>

                          <v-col cols="12" sm="6" md="6">
                            <v-text-field
                              v-model="detalleUsuario.nombre"
                             
                              label="Nombres"
                            ></v-text-field>
                          </v-col>

                          <v-col cols="12" sm="6" md="6">
                            <v-text-field
                              v-model="detalleUsuario.email"
                              label="E-mail"
                              
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6" md="6">
                            <v-text-field
                              v-model="detalleUsuario.password"
                              label="Password"
                              
                            ></v-text-field>
                          </v-col>
                        </v-row>
                        <v-card-actions>
                          <v-btn
                            color="Black lighten-2"
                            text
                            to="/Home"
                            class="mb-2 green accent-3"
                          >
                            Regresar
                          </v-btn>
                          <v-btn
                            class="mb-2 red darken-4"
                            color="Black lighten-2"
                            text
                            @click="editarItem(detalleUsuario._id)"
                          >
                            Guardar
                          </v-btn>
                        </v-card-actions>
                      </v-container>
                    </template>
                  </v-card-text>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

  
<script>
import axios from "axios";
export default {
  name: "pagesPerfil",
 data: () => ({
    
    detalleUsuario: [],
  }),
  methods: {
    traerUsuario() {
      this.detalleUsuario = this.$store.state.datos;
      console.log(this.detalleUsuario);
    },
    editarItem(id) {
      if (id) {
        axios
          .put(`https://back-coohilados.vercel.app/api/usuario/${this.id}`, {
            documento: this.detalleUsuario.documento,
            nombre: this.detalleUsuario.nombre,
            email: this.detalleUsuario.email,
            password: this.detalleUsuario.password,
            
          })
          .then((response) => {
            this.traerUsuario();
            this.dialog = false;
            console.log(response);
            this.$store.dispatch("setDatos", response.data.item);
            this.$router.push("/Verusuario");
            this.loading = false;
            this.$swal({
              icon: "success",
              title: "Se edito Usuario correctamente",
            });
          })
          .catch((error) => {
            console.log(error);
            this.dialog = false;
            this.loading = false;
            this.$swal({
              icon: "error",
              title:
                "Error al editar el Usuario, por favor verifique los datos",
            });
          });
      }
    },
  },
  created() {
    this.traerUsuario();
  },
};
</script>