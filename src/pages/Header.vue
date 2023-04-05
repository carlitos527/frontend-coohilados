<template>
  <div>
    <v-app-bar color="#07451E" v-if="navbar != 'oVer'">
      <v-app-bar-nav-icon class="white--text" @click="drawer = true"></v-app-bar-nav-icon><br /><br />
      <v-row>
        <v-col cols="1" v-if="$store.state.token != undefined">
          <v-list-item to="/Home">
            <v-list-item-icon class="white--text mb-2 mt-10">
              <font-awesome-icon icon="fa-solid fa-house" />
            </v-list-item-icon>
          </v-list-item> </v-col><br />
        <v-col cols="7">
          <v-toolbar-title class="white--text mb-2 mt-7">
            <h3>COOHILADOS</h3>
            <br />
          </v-toolbar-title>
        </v-col>
      </v-row>

      <v-btn class="white--text mx-6" icon v-if="$store.state.token != undefined" @click="cerrarSesion()">
        <v-icon>mdi-account-off</v-icon>
      </v-btn>
      <v-menu left bottom> </v-menu>
      <div class="mx-6">
        <v-btn color="orange darken-2" fab dark to="/" v-if="$store.state.token === undefined">
          <v-icon>mdi-account-circle</v-icon>
        </v-btn>
      </div>
    </v-app-bar>

    <template>
      <div></div>
    </template>

    <v-navigation-drawer
      src="https://eurofique.info/wp-content/gallery/visita-virtual-expo-fique-historia-y-futuro/07-saco-de-fique.jpg"
      v-model="drawer" absolute temporary v-if="$store.state.token != undefined">
      <v-img src="https://coohilados.com.co/carga/upload/images/QUIENESSOMOS1.jpg">
        <v-list nav dense>
          <v-list-item-group color="orange darken-4" v-model="group" active-class="purple lighten-2">
            <h1>INICIO</h1>
            <v-list-group color="green accent-4" :value="true" no-action sub-group>
              <v-list-item color="red accent-3" to="Gerencia" v-if="$store.state.usuario.rol == 'Visualizador'">
                <h4>CONSEJO O GERENCIA</h4>
              </v-list-item>
              <v-list-item color="green accent-4" to="TH" v-if="$store.state.usuario.rol == 'Editor de Datos'">
                <h3>TALENTO HUMANO</h3>
              </v-list-item>
              <v-list-item color="red accent-3" to="Sistemas" v-if="$store.state.usuario.rol == 'Administrador'">
                <h3>SISTEMAS</h3>
              </v-list-item>
              <v-list-item color="red accent-3" to="Sst" v-if="$store.state.usuario.rol == 'Actualizador'">
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
    navbar: ""
  }),
  methods: {
    cerrarSesion() {
      this.$store.state.token == undefined
      this.$router.push("/");
      localStorage.removeItem("token");
      localStorage.removeItem("usuario");
      localStorage.setItem('nav', 'noVer')
      /* localStorage.clear(); */
      this.$swal({
        icon: "success",
        title: "Usted cerro sesión exitosamente. 👋",
      });
    },
    traerUsuario() {
      this.persona = this.$store.state.usuario;
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
    },
    traer() {
      this.navbar = localStorage.getItem('nav');
      console.log(this.navbar);
    }
  },
  created() {
    this.traerUsuario();
    this.traer();
    console.log("created de header");
  },
  mounted(){
    console.log("mounted de header");
  }
};
</script>