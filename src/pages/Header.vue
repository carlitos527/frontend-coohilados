<template>
  <div>
    <v-app-bar color="#07451E">
      <v-app-bar-nav-icon
        class="white--text"
        @click="drawer = true"
        v-if="this.$store.state.token !=undefined"
      ></v-app-bar-nav-icon
      ><br /><br />
      <v-row>
        <v-col cols="1">
          <v-list-item to="/Home" v-if="this.$store.state.token !=undefined">
            <v-list-item-icon class="white--text mb-2 mt-10">
              <font-awesome-icon icon="fa-solid fa-house" />
            </v-list-item-icon>
          </v-list-item> </v-col
        ><br />
        <v-col cols="7">
          <v-toolbar-title class="white--text mb-2 mt-7"
            ><h3>COOHILADOS</h3>
            <br />
          </v-toolbar-title>
        </v-col>
        <v-col
          ><p class="black--text mb-2 mt-4">{{ this.$store.state.usuario.nombre }}</p></v-col
        >
      </v-row>

      <!-- <v-btn icon>
        <v-icon class="white--text" @click="buscar()">mdi-magnify</v-icon>
      </v-btn> -->

      <v-btn
        class="white--text mx-6"
        icon
        v-if="this.$store.state.token !=undefined"
        @click="cerrarSesion()"
      >
        <v-icon>mdi-account-off</v-icon>
      </v-btn>
      <v-menu left bottom> </v-menu>
      <div class="mx-6">
        <v-btn
          color="orange darken-2"
          fab
          dark
          to="/"
          v-if="this.$store.state.token ===undefined"
        >
          <v-icon>mdi-account-circle</v-icon>
        </v-btn>
      </div>
    </v-app-bar>

    <template>
      <div></div>
    </template>

    <v-navigation-drawer
      src="https://eurofique.info/wp-content/gallery/visita-virtual-expo-fique-historia-y-futuro/07-saco-de-fique.jpg"
      v-model="drawer"
      absolute
      temporary
      v-if="$store.state.token !=undefined"
    >
      <v-img
        src="https://coohilados.com.co/carga/upload/images/QUIENESSOMOS1.jpg"
      >
        <v-list nav dense>
          <v-list-item-group
            color="orange darken-4"
            v-model="group"
            active-class="purple lighten-2"
          >
            <h1>INICIO</h1>

            <v-list-group
              color="green accent-4"
              :value="true"
              no-action
              sub-group
            >
              <v-list-item color="red accent-3" to="Gerencia">
                <h4>CONSEJO O GERENCIA</h4>
              </v-list-item>

               <v-list-item color="green accent-4" to="TH">
                <h3>TALENTO HUMANO</h3>
              </v-list-item>

              <v-list-item color="red accent-3" to="Sistemas">
                <h3>SISTEMAS</h3>
              </v-list-item>

              <v-list-item color="red accent-3" to="Sst">
                <h3>SST</h3>
              </v-list-item>
            


            </v-list-group>
            
             
          </v-list-item-group>
        </v-list>
      </v-img>
    </v-navigation-drawer>
  </div>
</template>
<script>
export default {
  name: "PageHeader",
  data: () => ({
    drawer: false,
    group: null,
    cerrar: "",
    persona: "",
  }),
  methods: {
    cerrarSesion() {
      this.$store.state.token=undefined

      this.$router.push("/");
      console.log("cerrarSesion");
      localStorage.removeItem("token");
      localStorage.removeItem("usuario")

      this.$swal({
            icon: "success",
            title: "Usted cerro sesión exitosamente. 👋",
          });
    },
    traerUsuario() {
      this.persona = this.$store.state.usuario;
      console.log(this.persona);
      console.log(this.$store.state.usuario);
    },
  },
  created() {
    this.traerUsuario();
  },
};
</script>