<template>
  <v-app>
    <v-container fluid >
      <v-row class="justify-center align-center" style="height:90vh">
        <v-col class="text-center" col="12" sm="6">
          <v-card style="background-color:#ffe082">
            <v-card-title class="text-h5"><h1> Ingrese nuevo usuario</h1></v-card-title>
            <v-card-text>
              <v-text-field
                v-model="documento"
                :rules="documentoRules"
                label="Documento"
                required
              ></v-text-field>

              <v-text-field
                v-model="nombre"
                label="Nombre y Apellidos"
                :rules="nombreRules"
                required
              ></v-text-field>

              <v-text-field
                v-model="email"
                label="E-mail"
                :rules="emailRules"
                required
              ></v-text-field>
              <v-text-field
                v-model="password"
                label="Escriba una Contraseña"
                :rules="passwordRules"
                required
              ></v-text-field>

              <v-select
                v-model="area"
                label="Area"
                :items="area"
                required
              ></v-select>


              <v-select
                v-model="rol"
                label="Rol"
                :items="rol"
                required
              ></v-select>
            </v-card-text>
            <v-card-actions>
              <v-btn color="blue darken-1" text to="/">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="agregar()"
                >Guardar</v-btn
              >
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>
<script>
import axios from "axios";
export default {
  name: "PagesAgregarusuario",
  data: () => ({
    loading: false,
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
    passwordlRules: [
      (p) => !!p || " La contraseña es requerido ❌",
      (p) =>
        (p && p.length <= 20) ||
        " La contraseña  no puede tener menos de 8 caracteres",
    ],
    valid7: true,

    area: [
      "CONSEJO O GERENCIA",
      "TALENTO HUMANO",
      "SISTEMAS",
      "SST",
    ],
     rol: [
      "Actualizador",
      "Administrador",
      "Editor de Datos",
      "Visualizador",
    ],

    usuarios: [],
  }),
  methods: {
    detalleUsuario(item) {
      this.$router.push("/Infousuario");
      this.$store.dispatch("setDatos", item);
      console.log(this.$store.state.datos);
    },

    traerUsuarios() {
      axios
        .get("https://back-coohilados.vercel.app/api/usuario")
        .then((response) => {
          this.usuarios = response.data.usuario;
          console.log(this.usuarios);
        })
        .catch((err) => {
          console.log(err);
        });
    },

    desactivar(id) {
      console.log(id);
      if (id) {
        axios
          .put(
            `https://back-coohilados.vercel.app/api/usuario//desactivar/desactivar/${id}`
          )
          .then((response) => {
            console.log(response);
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
    activar(id) {
      console.log(id);
      if (id) {
        axios
          .put(`https://back-coohilados.vercel.app/api/usuario/activar/${id}`)
          .then((response) => {
            console.log(response);
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
    agregar() {
      this.loading = true;
      let header = { headers: { "x-token": this.$store.state.token } };
      console.log(header);
      axios
        .post(
          "https://back-coohilados.vercel.app/api/usuario",
          {
            documento: this.documento,
            nombre: this.nombre,
            email: this.email,
            password: this.password,
            rol: this.rol,
          },
          header
        )
        .then((response) => {
          this.dialog = false;
          console.log(response);
          this.$store.dispatch("setDatos", response.data.item);
          this.$router.push("/");
          this.loading = false;

          this.$swal({
            icon: "success",
            title:
              "Se creo el Usuario  correctamente 👌, por favor digite su correo y contraseña ⌨️",
          });
        })
        .catch((error) => {
          console.log(error);
          this.dialog = false;
          this.loading = false;
          this.$swal({
            icon: "error",
            title:
              "Error al guardar el Usuario, Por favor verifique los datos correctamente los datos",
          });
        });
    },
  },

  created() {
    this.traerDepartamentos();
    //this.traerCiudades()
    //this.agregar()
    this.traerUsuarios();
  },
};
</script>