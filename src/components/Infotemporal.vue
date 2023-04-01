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
                <v-card-title>{{ detalleTemporal.nombre }}</v-card-title>
              </v-col>
            </v-card-title>

            <v-card-text>
              <v-row align="center" class="mx-0">
                <v-col cols="12" sm="6" md="4">
                  <p label="Estado" v-if="detalleTemporal.estado == 1">
                    Estado: Activo
                  </p>
                  <p label="Estado" v-if="detalleTemporal.estado == 2">
                    Estado: Inactivo
                  </p>
                  <p label="Estado" v-if="detalleTemporal.estado == 3">
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
                      <v-select
                        v-model="detalleTemporal.tipoDocumento"
                        :items="tDocumento"
                        label="Tipo de Documento"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTemporal.documento"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                        label="Documento"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTemporal.nombre"
                        label="Nombre"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="detalleTemporal.sexo"
                        :items="sexoArray"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                        label="Sexo"
                      ></v-select>
                    </v-col>
                    <v-menu
                      v-model="menu2"
                      v-if="$store.state.usuario.rol == 'Editor de Datos'"
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
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                        @input="menu2 = false"
                        @change="cambioN"
                      ></v-date-picker>
                    </v-menu>

                    <v-menu
                      v-model="menu3"
                      :close-on-content-click="false"
                      :nudge-right="40"
                      transition="scale-transition"
                      offset-y
                      min-width="auto"
                      v-if="$store.state.usuario.rol == 'Editor de Datos'"
                    >
                      <template v-slot:activator="{ on, attrs }"
                      v-if="$store.state.usuario.rol == 'Editor de Datos'">
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
                        @change="cambioI"
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
                      <template v-slot:activator="{ on, attrs }"
                      v-if="$store.state.usuario.rol == 'Editor de Datos'">
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
                        @change="cambioF"
                      ></v-date-picker>
                    </v-menu>

                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        :items="area"
                        v-model="detalleTemporal.areaTrabajo"
                        label="Área de trabajo"
                        item-text="nombre"
                        item-value="_id"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        :items="rolArray"
                        v-model="detalleTemporal.rol"
                        label="Cargo"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTemporal.salario"
                        label="Salario"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTemporal.barrio"
                        label="Direccion"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-text-field>
                    </v-col>
                    <v-select
                      :items="cities"
                      v-model="departamento"
                      label="Departamento"
                      @change="traerCiudades()"
                      v-if="$store.state.usuario.rol == 'Editor de Datos'"
                    >
                    </v-select>
                    <v-select
                      :items="town"
                      v-model="detalleTemporal.city"
                      item-text="Ciudad"
                      item-value="_id"
                      label="Ciudad"
                      @change="prueba()"
                      v-if="$store.state.usuario.rol == 'Editor de Datos'"
                    >
                    </v-select>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTemporal.telefono"
                        label="Telefono"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-text-field>
                    </v-col>
                    <v-col>

                    <v-text-field
                        v-model="detalleTemporal.email"
                        label="E-mail"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTemporal.anotacion"
                        label="ANOTACIÓN"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </template>
            </v-card-text>

            <v-card-actions>
              <v-btn color="deep-purple lighten-2" text to="/AgregarTemporales">
                Regresar
              </v-btn>
              <v-btn color="deep-purple lighten-2" text @click="editarItem()">
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
  name: "PageInfotemporal",

  data: () => ({
    loading: false,
    menu3: false,
    menu4: false,
    menu2: false,
    fechaNacimiento: new Date(
      Date.now() - new Date().getTimezoneOffset() * 60000
    )
      .toISOString()
      .substr(0, 10),
    fechaInicio: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    fechaFin: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
      
    rolRules: [(r) => !!r || " El Cargo es requerido ❌"],
    rolArray: [
      "ASISTENTE AGRICOLA",
      "ASISTENTE DIRECTOR COMERCIAL",
      "AUXILIAR ADMINISTRATIVO",
      "AUXLIAR COMERCIAL",
      "AUXILIAR CONTABLE",
      "AUXILIAR CONTABLE II",
      "AUXILIAR CONTROL DE CALIDAD",
      "AUXILIAR PRODUCCION",
      "BODEGUERO",
      "BODEGUERO P.T",
      "BODEGUERO M.P",
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
    sexoArray: ["F", "M"],
    area: [],
    cities: [],
    town: [],
    tDocumento:["C.C","C.E"],
    detalleTemporal: {
      tipoDocumento: "",
      documento: "",
      nombre: "",
      areaTrabajo: "",
      salario: "",
      barrio: "",
      city: "",
      telefono: "",
      anotacion: "",
      sexo: "",
      rol: "",
      fechaN: "",
      fechaI: "",
      fechaF: "",
      
    },
    id: "",
  }),
  methods: {
    cambioN() {
      console.log("cambio la fecha de nacimiento: ");
      this.detalleTemporal.fechaN = this.fechaNacimiento;
    },
    cambioI() {
      console.log("cambio la fecha de inicio: ");
      this.detalleTemporal.fechaI = this.fechaInicio;
    },
    cambioF() {
      console.log("cambio la fecha fin: ");
      this.detalleTemporal.fechaF = this.fechaFin;
    },
    traerAreaTrabajo() {
      axios
        .get("https://back-coohilados.vercel.app/api/areaTrabajo")
        .then((response) => {
          this.area = response.data.lugar;
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
        })
        .catch((err) => {
          console.log(err);
        });
    },
    traerTemporal() {
      this.id = this.$store.state.datos._id;
      this.detalleTemporal = {
        tipoDocumento: this.$store.state.datos.tipoDocumento,
        documento: this.$store.state.datos.documento,
        nombre: this.$store.state.datos.nombre,
        salario: this.$store.state.datos.salario,
        barrio: this.$store.state.datos.barrio,
        telefono: this.$store.state.datos.telefono,
        anotacion: this.$store.state.datos.anotacion,
        sexo: this.$store.state.datos.sexo,
        rol: this.$store.state.datos.rol,
        areaTrabajo: this.$store.state.datos.areaTrabajo._id,
        city: this.$store.state.datos.ciudad,
        fechaN: this.$store.state.datos.fechaNacimiento,
        email: this.$store.state.datos.email,
        fechaI: this.$store.state.datos.fechaInicio,
        fechaF: this.$store.state.datos.fechaFin,
      };
    },
    editarItem() {
      axios
        .put(
          `https://back-coohilados.vercel.app/api/ayudaTemporal/${this.id}`,
          {
            tipoDocumento: this.detalleTemporal.tipoDocumento,
            documento: this.detalleTemporal.documento,
            sexo: this.detalleTemporal.sexo,
            nombre: this.detalleTemporal.nombre,
            fechaNacimiento: this.detalleTemporal.fechaN,
            fechaInicio: this.detalleTemporal.fechaI,
            fechaFin: this.detalleTemporal.fechaF,
            areaTrabajo: this.detalleTemporal.areaTrabajo,
            salario: this.detalleTemporal.salario,
            barrio: this.detalleTemporal.barrio,
            ciudad: this.detalleTemporal.city,
            telefono: this.detalleTemporal.telefono,
            email: this.detalleTemporal.email,
            anotacion: this.detalleTemporal.anotacion,
            rol: this.detalleTemporal.rol,
          }
        )
        .then((response) => {
          this.traerTemporal();
          this.dialog = false;
          console.log(response);
          this.$store.dispatch("setDatos", response.data.item);
          this.$router.push("/AgregarTemporales");
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
    },
    fecha(r) {
      let d = new Date(r);
      let f = d.toISOString();
      return f.split("T")[0].replace(/-/g, "/");
    },
  },
  created() {
    this.traerTemporal();
    this.traerAreaTrabajo();
    this.traerDepartamentos();
  },
};
</script>