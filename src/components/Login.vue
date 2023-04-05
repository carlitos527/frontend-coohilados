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
            <h4 class="text-center">
                <router-link to="/Agregarusuario" color="black">
                  Crea un usuario!
                </router-link>
              </h4>

            <v-form ref="form" class="pa-4 pt-6">
              <v-text-field v-model="email" label="Usuario" type="text" outlined required hint="Correo Corporativo">
              </v-text-field>
              <v-text-field v-model="password" :counter="20" label="Password"
                :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'" :type="show1 ? 'text' : 'password'" outlined required
                @click:append="show1 = !show1"></v-text-field>

              
              <v-card-action>
                <v-flex class="text-center">
                  <v-btn rounded class="ma-2" outlined color="green" @click="login()">
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
            <v-progress-circular v-show="loading == true" :size="70" :width="7" color="black"
              indeterminate></v-progress-circular>
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
    show1: false,
    tiempo: 900
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
          /* localStorage.setItem('nav','siVer') */
          this.$router.push("/Home");
          this.$swal({
            icon: "success",
            title: "Inicio de sesión exitoso",
            
          });
          this.expirarSesion();
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
          this.$swal({
            icon: "error",
            title: "Error al iniciar sesión, el usuario probablemente no existe",
          });
        });
    },
    expirarSesion() {
      let i = setInterval(() => {
        window.onmousemove = () => {
          this.tiempo = 900
        }
        this.tiempo--
        console.log(this.tiempo);
        if (this.tiempo == 450) {
          this.$swal({
            icon: "info",
            title: `Su sesión esta a 5 segundos de cerrar`,
          });
        }
        if (this.tiempo == -1) {
          this.$store.state.token = undefined
          this.$router.push("/");
          localStorage.removeItem("token");
          localStorage.removeItem("usuario")
          this.$swal({
            icon: "info",
            title: "Su sesión a expirado",
          });
          clearInterval(i)
        }
      }, 1200)
    }
  },
  created(){
    console.log("created de login");
  }
};
</script>