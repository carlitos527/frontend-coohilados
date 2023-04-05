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
                        <h3 class="mb-0">
                          {{ `Área: ${$store.state.usuario.area}` }}
                        </h3>
                        <h3 class="mb-0">
                          {{ `Rol: ${$store.state.usuario.rol}` }}
                        </h3>
                        <h3 v-if="$store.state.usuario.estado == 1">
                          Estado:
                          <p label="Estado" style="color: green">🟢</p>
                        </h3>
                        <h3 v-if="$store.state.usuario.estado == 2">
                          Estado:
                          <p label="Estado" style="color: red">🔴</p>
                        </h3>
                        <h3 v-if="$store.state.usuario.estado == 3">
                          Estado:
                          <p label="Estado" style="color: orange">🟠</p>
                        </h3>
                      </v-col>
                      <v-card-title justify-right>
                        <v-card-title>{{
                          `${$store.state.usuario.nombre}`
                        }}</v-card-title>
                      </v-card-title>
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
                              :rules="documentoRules"
                              label="Documento"
                            ></v-text-field>
                          </v-col>

                          <v-col cols="12" sm="6" md="6">
                            <v-text-field
                              v-model="detalleUsuario.nombre"
                              :rules="nombreRules"
                              label="Nombres"
                            ></v-text-field>
                          </v-col>

                          <v-col cols="12" sm="6" md="6">
                            <v-text-field
                              v-model="detalleUsuario.email"
                              label="E-mail"
                              :rules="emailRules"
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
                            rounded
                            outlined
                            color="black"
                            class="mb-2 red accent-3"
                            text
                            to="/Home"
                          >
                            Regresar
                          </v-btn>
                          <v-btn
                            rounded
                            outlined
                            color="black"
                            class="mb-2 green accent-3"
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
          <v-row class="align-center">
            <v-col>
              <v-overlay :value="loading">
                <v-progress-circular
                  v-show="loading == true"
                  :size="70"
                  :width="7"
                  color="black"
                  indeterminate
                ></v-progress-circular>
              </v-overlay>
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
    loading: false,
    show1: false,

    dialog: false,
    dialogEdit: false,
    valid: true,
    nombre: "",
    nombreRules: [
      (n) => !!n || " Nombre y Apellidos son requerido ❌",
      (n) =>
        (n && n.length <= 50) || " EL Nombre solo puede tener 50 caracteres",
    ],
    vali2: true,
    documento: "",
    documentoRules: [
      (d) => !!d || " EL numero del Documento es requerido ❌",
      (d) =>
        (d && d.length <= 17) || " Cedula No puede tener menos de 6 caracteres",
    ],
    vali5: true,
    email: "",
    emailRules: [
      (e) => !!e || " EL E-mail es requerido ❌",
      (e) =>
        (e && e.length <= 40) ||
        " el email  solo puede tener menos de  40 caracteres",
    ],
    valid6: true,
    password: "",

    usuarios: [],
    detalleUsuario: {
      documento: "",
      nombre: "",
      email: "",
      password: "",
    },
    id: "",
  }),
  methods: {
    traerUsuario() {
      this.id = this.$store.state.usuario._id;
      this.detalleUsuario = {
        documento: this.$store.state.usuario.documento,
        nombre: this.$store.state.usuario.nombre,
        email: this.$store.state.usuario.email,
        password: this.$store.state.usuario.password,
      };
    },
    editarItem() {
      this.loading = true;
      axios
        .put(`https://back-coohilados.vercel.app/api/usuario/${this.id}`, {
          documento: this.detalleUsuario.documento,
          nombre: this.detalleUsuario.nombre,
          email: this.detalleUsuario.email,
          password: this.detalleUsuario.password,
        })
        .then((response) => {
          this.traerUsuario();
          (this.loading = false), console.log(response);
          this.$store.dispatch("setDatos", response.data.item);
          this.$router.push("/Perfil");

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
            title: "Error al editar el Usuario, por favor verifique los datos",
          });
        });
    },
    estado() {},
  },

  created() {
    this.traerUsuario();
  },
};
</script>