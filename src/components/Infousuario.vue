<template>
  <v-card class="mx-auto my-12" max-width="500">
    <template slot="progress">
      <v-progress-linear
        color="deep-purple"
        height="10"
        indeterminate
      ></v-progress-linear>
    </template>

    <v-card-title justify-right> </v-card-title>

    <v-card-text>
      <v-row align="center" class="mx-0">
        <v-col cols="12" sm="6" md="4">
          <p label="Estado" v-if="this.$store.state.datos.estado == 1">
            Estado: Activo
          </p>
          <p label="Estado" v-if="this.$store.state.datos.estado == 2">
            Estado: Inactivo
          </p>
          <p label="Estado" v-if="this.$store.state.datos.estado == 3">
            Estado: De vacaciones
          </p>
        </v-col>
      </v-row>
    </v-card-text>

    <v-divider class="mx-4"></v-divider>

    <v-card-title>{{ detalleUsuario.nombre }}</v-card-title>

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
              <v-select
                v-model="detalleUsuario.area"
                label="Area"
                :items="area"
                required
              ></v-select>
            </v-col>

            <v-col cols="12" sm="6" md="6">
              <v-select
                v-model="detalleUsuario.rol"
                label="Rol"
                :items="rol"
                required
              ></v-select>
            </v-col>
          </v-row>
        </v-container>
      </template>
    </v-card-text>

    <v-card-actions>
      <v-btn color="deep-purple lighten-2" text to="/Verusuario">
        Regresar
      </v-btn>
      <v-btn
        color="deep-purple lighten-2"
        text
        @click="editarItem(detalleUsuario._id)"
      >
        Guardar
      </v-btn>
    </v-card-actions>
  </v-card>
</template>
<script>
import axios from "axios";
export default {
  name: "PageInfousuario",
  data: () => ({
    area: ["CONSEJO O GERENCIA", "TALENTO HUMANO", "SISTEMAS", "SST"],
    rol: ["Actualizador", "Administrador", "Editor de Datos", "Visualizador"],
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
            area: this.detalleUsuario.area,
            rol: this.detalleUsuario.rol,
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