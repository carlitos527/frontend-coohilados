<template>
  <v-app>
    <v-container class="m3">
      <v-row>
        <v-col>
          <v-card class="mx-auto" elevation="14" style="max-width: 800px">
            <v-toolbar center flat dark class="green lighten-2">
              <v-toolbar-title class="mx-auto font-italic mb-2">
                <h2>Ingresar</h2>
              </v-toolbar-title>
            </v-toolbar>

            <v-form ref="form" class="pa-4 pt-6">
              <v-text-field
                v-model="email"
                label="Usuario"
                type="text"
                outlined
                required
                hint="Correo Corporativo"
              >
              </v-text-field>
              <v-text-field
                v-model="password"
                :counter="10"
                label="Password"
                type="password"
                outlined
                required
              ></v-text-field>

              <h4 class="text-center">
                <router-link to="/Agregarusuario" color="black">
                  Crea un usuario!
                </router-link>
              </h4>
              <v-card-action>
                <v-flex class="text-center">
                  <v-btn
                    rounded
                    class="ma-2"
                    outlined
                    color="green"
                    @click="login()"
                  >
                    Ingresar
                  </v-btn>
                </v-flex>
              </v-card-action>
            </v-form>
          </v-card>
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
    </v-container>
  </v-app>
</template>
  
<script>
import axios from "axios";
export default {
  name: "PageLogin",

  data: () => ({
    email: "",
    password: "",
    loading: false,
  }),
  methods: {
    login() {
      this.loading = true;
      axios
        .post("https://back-coohilados.vercel.app/api/usuario/login", {
          email: this.email,
          password: this.password,
        })
        .then((response) => {
          console.log(response.data.token);
          this.$store.dispatch("setToken", response.data.token);
          this.$store.dispatch("setDatosUsuario", response.data.usuario);
          this.$router.push("/Home");

          this.$swal({
            icon: "success",
            title: "Inicio de sesión exitoso",
          });
        })
        .catch((error) => {
          console.log(error);
          this.loading=false;
          this.$swal({
            icon: "error",
            title: "Error al iniciar sesión, el usuario probablemente no existe",
          });
        });
    },
  },
};
</script>