<template>
  <v-container fluid>
    <v-row>
      <v-col>
        <v-img
          src="https://www.coohilados.com.co/gestion/uploads/product/69/picture.jpg"
        >
          <v-card class="mx-auto my-12 yellow lighten-4" max-width="500">
            <template slot="progress">
              <v-progress-linear
                color="deep-purple"
                height="10"
                indeterminate
              ></v-progress-linear>
            </template>

            <!-- inicio de la cart del usuario a editar -->
            <v-card-title justify-right>
              <v-col cols="12" sm="6" md="6" justify="right">
                <v-card-title>{{ detalleDirecto.nombre }}</v-card-title>
              </v-col>
            </v-card-title>

            <v-card-text>
              <v-row align="center" class="mx-0">
                <v-col cols="12" sm="6" md="4">
                  <p label="Estado" v-if="detalleDirecto.estado == 1">
                    Estado: Activo
                  </p>
                  <p label="Estado" v-if="detalleDirecto.estado == 2">
                    Estado: Inactivo
                  </p>
                  <p label="Estado" v-if="detalleDirecto.estado == 3">
                    Estado: De vacaciones
                  </p>
                </v-col>
              </v-row>
            </v-card-text>

            <v-divider class="mx-4"></v-divider>

            <v-card-text>
              <template>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleDirecto.tipoDocumento"
                        label="Tipo de Documento"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleDirecto.documento"
                        label="Documento"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleDirecto.nombre"
                        label="Nombre"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="detalleDirecto.sexo"
                        :items="sexo"
                        label="Sexo"
                      ></v-select>
                    </v-col>

                    <v-menu
                      v-model="menu2"
                      :close-on-content-click="false"
                      :nudge-right="40"
                      transition="scale-transition"
                      offset-y
                      min-width="auto"
                    >
                      <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                          v-model="fechaNacimiento"
                          label="Escoja la Fecha de Nacimiento"
                          prepend-icon="mdi-calendar"
                          readonly
                          v-bind="attrs"
                          v-on="on"
                        ></v-text-field>
                      </template>

                      <v-date-picker
                        v-model="fechaNacimiento"
                        @input="menu2 = false"
                      ></v-date-picker>
                    </v-menu>

                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="detalleDirecto.tipoContrato"
                        :items="tipoContrato"
                        label="Tipo de contrato"
                      ></v-select>
                    </v-col>

                    <v-menu
                      v-model="menu3"
                      :close-on-content-click="false"
                      :nudge-right="40"
                      transition="scale-transition"
                      offset-y
                      min-width="auto"
                    >
                      <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                          v-model="fechaInicio"
                          label="Escoja la Fecha de inicio de contrato"
                          prepend-icon="mdi-calendar"
                          readonly
                          v-bind="attrs"
                          v-on="on"
                        ></v-text-field>
                      </template>
                      <v-date-picker
                        v-model="fechaInicio"
                        @input="menu3 = false"
                      ></v-date-picker>
                    </v-menu>
                    <v-menu
                      v-model="menu4"
                      :close-on-content-click="false"
                      :nudge-right="40"
                      transition="scale-transition"
                      offset-y
                      min-width="auto"
                    >
                      <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                          v-model="fechaFin"
                          label="Escoja la Fecha de finalización de contrato"
                          prepend-icon="mdi-calendar"
                          readonly
                          v-bind="attrs"
                          v-on="on"
                        ></v-text-field>
                      </template>
                      <v-date-picker
                        v-model="fechaFin"
                        @input="menu4 = false"
                      ></v-date-picker>
                    </v-menu>

                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        :items="area"
                        v-model="detalleDirecto.areaTrabajo.nombre"
                        label="Área de trabajo"
                        item-text="nombre"
                        item-value="_id"
                      ></v-select>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        :items="rol"
                        v-model="detalleDirecto.rol"
                        label="Cargo"
                      ></v-select>
                    </v-col>
                    
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleDirecto.salario"
                        label="Salario"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleDirecto.barrio"
                        label="Direccion"
                      ></v-text-field>
                    </v-col>

                    <v-select
                      :items="cities"
                      v-model="departamento"
                      label="Departamento"
                      @change="traerCiudades()"
                    ></v-select>
                    <v-select
                      :items="town"
                      v-model="city"
                      item-text="Ciudad"
                      item-value="_id"
                      label="Ciudad"
                      @change="prueba()"
                    ></v-select>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleDirecto.telefono"
                        label="Telefono"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleDirecto.email"
                        label="E-mail"
                      ></v-text-field>
                    </v-col>

                     

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleDirecto.anotacion"
                        label="ANOTACIÓN"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </template>
            </v-card-text>

            <v-card-actions>
              <v-btn color="deep-purple lighten-2" text to="/AgregarDirecto">
                Regresar
              </v-btn>
              <v-btn
                color="deep-purple lighten-2"
                text
                @click="editarItem(detalleDirecto._id)"
              >
                Guardar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-img>
      </v-col>
    </v-row>
  </v-container>
</template>


<script>
import axios from "axios";
export default {
  name: "PagesInfodirecto",

  data: () => ({
    loading: false,
    fechaInicio: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    fechaFin: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    fechaNacimiento: new Date(
      Date.now() - new Date().getTimezoneOffset() * 60000
    )
      .toISOString()
      .substr(0, 10),
    menu3: false,
    menu4: false,
    menu2: false,
    tipoContrato:[ "PRESTACIÓN SERVICIO","TERMINO INDEFINIDO", "TERMINO FIJO", "OBRA O LABOR"],
    sexo: ["F", "M"],

     rol: [
    
    "ASISTENTE AGRICOLA",
    "ASISTENTE DIRECTOR COMERCIAL",
    "AUXILIAR ADMINISTRATIVO",
    "AUXLIAR COMERCIAL",
    "AUXILIAR CONTABLE",
    "AUXILIAR CONTABLE II",
    "AUXILIAR CONTROL DE CALIDAD",
    "AUXILIAR PRODUCCION",
    "BODEGUERO","BODEGUERO P.T","BODEGUERO M.P",
    "COMPRADOR ALMACENISTA",
    "COORDINADOR DE PRODUCCIÓN",
    "COORDINADOR DE SST",
    "DIRECTOR COMERCIAL",
    "DIRECTOR CONTABLE",
    "GERENTE GENERA",
    "INGENIERO DE SISTEMAS",
    "JEFE DE BODEGA DE PRODUCTOS TERMINADOS",
    "JEFE DE COMPRAS DE MATERIA PRIMA",
    "JEFE DE GESTION HUMANA",
    "JEFE DE MANTENIMIENTO Y TALLER",
    "LIDER AUXILIAR DE COSTURA",
    "LIDER DE SECCIÓN DE COSTURA",
    "LIDER DE SECCIÓN HILOS",
    "LIQUIDADOR DE PRODUCCIÓN",
    "MENSAJERO",
    "NOMINA",
    "OPERARIO",
    "OPERARIO DE PLANTA",
    "OPERARIO HILADORAS-HUSOS",
    "OPERARIO DE PRENSA",
    "RECEPCIONISTA",
    "RECEPCIONISTA DEL SGC",
    "SECRETARIO CONSEJO",
    "SECRETARIA GERENCIA",
    "SERVICIOS GENERALES",
    "TECNICO MANTENIMIENTO-CUADRILLA",
    "TECNICO MANTENIMIENTO-CUADRILLA-E",
    "TECNICO MANTENIMIENTO-BARRAS",
    "TECNICO MANTENIMIENTO-EBANISTA",
    "TECNICO MANTENIMIENTO-FRESADOR",
    "TECNICO MANTENIMIENTO-TORNERO",

    ],
    rolRules: [
      (r) => !!r || " El Cargo es requerido ❌",
    ],
    area: [],
    cities: [],
    town: [],
    detalleDirecto: [],
    city:""
  }),
  methods: {
    traerAreaTrabajo() {
      axios
        .get("https://back-coohilados.vercel.app/api/areaTrabajo")
        .then((response) => {
          // response.data.ciudad.reduce((obj, item) => (obj[item.Departamento] = true, obj), {});
          this.area = response.data.lugar;
          console.log(this.area);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    traerDepartamentos() {
      axios
        .get("https://back-coohilados.vercel.app/api/ciudad/departamento/get")
        .then((response) => {
          // response.data.ciudad.reduce((obj, item) => (obj[item.Departamento] = true, obj), {});
          this.cities = response.data.departamentos;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    traerCiudades() {
      axios
        .get(
          `https://back-coohilados.vercel.app/api/ciudad/ciudad/get/${this.departamento}`
        )
        .then((response) => {
          //this.town = response.data.city.reduce((obj, item) => (obj[item.Ciudad] = true, obj), {});
          //this.ciudad = response.data.city.filter(c => { return c.Ciudad.length > 3})
          this.town = response.data.city;
          console.log(this.town);
        })
        .catch((err) => {
          console.log(err);
        });
    },

    traerDirecto() {
      this.detalleDirecto = this.$store.state.datos;
      console.log(this.detalleDirecto);
    },
    editarItem(id) {
      if (id) {
        axios
          .put(
            `https://back-coohilados.vercel.app/api/trabajadorDirecto/${id}`,
            {
              tipoDocumento: this.detalleDirecto.tipoDocumento,
              documento: this.detalleDirecto.documento,
              sexo: this.detalleDirecto.sexo,
              nombre: this.detalleDirecto.nombre,
              fechaNacimiento: this.detalleDirecto.fechaNacimiento,
              tipoContrato: this.detalleDirecto.tipoContrato,
              fechaInicio: this.detalleDirecto.fechaInicio,
              fechaFin: this.detalleDirecto.fechaFin,
              areaTrabajo: this.detalleDirecto.areaTrabajo,
              salario: this.detalleDirecto.salario,
              barrio: this.detalleDirecto.barrio,
              departamento: this.detalleDirecto.departamento,
              ciudad: this.city,
              telefono: this.detalleDirecto.telefono,
              anotacion: this.detalleDirecto.anotacion,
              rol: this.detalleDirecto.rol,
            }
          )
          .then((response) => {
            this.traerDirecto();
            this.dialog = false;
            console.log(response);
            this.$store.dispatch("setDatos", response.data.item);
            this.$router.push("/AgregarDirecto");
            this.loading = false;

            this.$swal({
              icon: "success",
              title: "El trabajador se edito correctamente",
            });
          })
          .catch((error) => {
            console.log(error);
            this.dialog = false;
            this.loading = false;
            this.$swal({
              icon: "error",
              title: "Error al editar el trabajador",
            });
          });
      }
    },

    fecha(r) {
      let d = new Date(r);
      let f = d.toISOString();
      return f.split("T")[0].replace(/-/g, "/");
    },
  },
  created() {
    this.traerAreaTrabajo();
    this.traerDepartamentos();
    //this.traerCiudades()
    //this.agregar()
    this.traerDirecto();
  },
};
</script>