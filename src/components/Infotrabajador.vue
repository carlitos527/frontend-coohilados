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
                <v-card-title>{{ detalleTrabajador.nombre }}</v-card-title>
              </v-col>
            </v-card-title>

            <v-card-text>
              <v-row align="center" class="mx-0">
                <v-col cols="12" sm="6" md="4">
                  <p label="Estado" v-if="detalleTrabajador.estado == 1">
                    Estado: Activo
                  </p>
                  <p label="Estado" v-if="detalleTrabajador.estado == 2">
                    Estado: Inactivo
                  </p>
                  <p label="Estado" v-if="detalleTrabajador.estado == 3">
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
                    <v-col cols="12" sm="6" md="6"
                    v-if="$store.state.usuario.rol == 'Editor de Datos'">
                      <v-select
                        v-model="detalleTrabajador.tipoDocumento"
                        :items="tDocumento"
                        label="Tipo de Documento"
                        
                      ></v-select>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTrabajador.documento"
                        label="Documento"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTrabajador.nombre"
                        label="Nombre"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="detalleTrabajador.sexo"
                        :items="sexoArray"
                        label="Sexo"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-select>
                    </v-col>

                    <v-menu
                      v-model="menu2"
                      :close-on-content-click="false"
                      :nudge-right="40"
                      transition="scale-transition"
                      offset-y
                      min-width="auto"
                      v-if="$store.state.usuario.rol == 'Editor de Datos'"
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
                        v-model="detalleTrabajador.tipoContrato"
                        :items="tContrato"
                        label="Tipo de contrato"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-select>
                    </v-col>

                    <v-menu
                      v-model="menu3"
                      :close-on-content-click="false"
                      :nudge-right="40"
                      transition="scale-transition"
                      offset-y
                      min-width="auto"
                      v-if="$store.state.usuario.rol == 'Editor de Datos'"
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
                      v-if="$store.state.usuario.rol == 'Editor de Datos'"
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
                        :items="rolArray"
                        v-model="detalleTrabajador.rol"
                        label="Cargo"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-select>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        :items="area"
                        v-model="detalleTrabajador.areaTrabajo"
                        label="Área de trabajo"
                        item-text="nombre"
                        item-value="_id"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTrabajador.salario"
                        label="Salario"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTrabajador.barrio"
                        label="Direccion"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-text-field>
                    </v-col>

                    <v-select
                      v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      :items="cities"
                      v-model="departamento"
                      label="Departamento"
                      @change="traerCiudades()"
                    ></v-select>
                    <v-select
                      v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      :items="town"
                      v-model="detalleTrabajador.city"
                      item-text="Ciudad"
                      item-value="_id"
                      label="Ciudad"
                      @change="prueba()"
                    ></v-select>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTrabajador.telefono"  
                        label="Telefono"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTrabajador.email"
                        label="E-mail"
                        v-if="$store.state.usuario.rol == 'Editor de Datos'"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="detalleTrabajador.anotacion"
                        label="ANOTACIÓN"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </template>
            </v-card-text>
            <v-card-actions>
              <v-btn
                color="deep-purple lighten-2"
                text
                to="/AgregarTrabajadores"
              >
                Regresar
              </v-btn>
              <v-btn
                color="deep-purple lighten-2"
                text
                @click="editarItem()"
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
  name: "PageInfotrabajador",

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

    tDocumento: ["C.C", "C.E"],

    tContrato: ["TERMINO FIJO", "TERMINO INDEFINIDO"],

    sexoArray: ["F", "M"],

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
    rolRules: [(r) => !!r || " El Cargo es requerido ❌"],

    
    area: [],
    cities: [],
    town: [],
    detalleTrabajador: {
      tipoDocumento: "",
      documento: "",
      nombre: "",
      areaTrabajo: "",
      tipoContrato: "",
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
  computed: {},
  methods: {
    cambioN() {
      console.log("cambio la fecha de nacimiento: ");
      this.detalleTrabajador.fechaN = this.fechaNacimiento;
    },
    cambioI() {
      console.log("cambio la fecha de inicio: ");
      this.detalleTrabajador.fechaI = this.fechaInicio;
    },
    cambioF() {
      console.log("cambio la fecha fin: ");
      this.detalleTrabajador.fechaF = this.fechaFin;
    },

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
    traerTrabajador() {
      this.id = this.$store.state.datos._id;
      this.detalleTrabajador = {
        tipoDocumento: this.$store.state.datos.tipoDocumento,
        documento: this.$store.state.datos.documento,
        nombre: this.$store.state.datos.nombre,
        tipoContrato:this.$store.state.datos.tipoContrato,
        salario: this.$store.state.datos.salario,
        barrio: this.$store.state.datos.barrio,
        telefono: this.$store.state.datos.telefono,
        email: this.$store.state.datos.email,
        anotacion: this.$store.state.datos.anotacion,
        sexo: this.$store.state.datos.sexo,
        rol: this.$store.state.datos.rol,
        areaTrabajo: this.$store.state.datos.areaTrabajo._id,
        city: this.$store.state.datos.ciudad,
        fechaN: this.$store.state.datos.fechaNacimiento,
        fechaI: this.$store.state.datos.fechaInicio,
        fechaF: this.$store.state.datos.fechaFin,
      };
    },
    editarItem() {
        axios
          .put(`https://back-coohilados.vercel.app/api/servicio/${this.id}`, 
          {
            tipoDocumento: this.detalleTrabajador.tipoDocumento,
            documento: this.detalleTrabajador.documento,
            sexo: this.detalleTrabajador.sexo,
            nombre: this.detalleTrabajador.nombre,
            fechaNacimiento: this.detalleTrabajador.fechaNacimiento,
            tipoContrato: this.detalleTrabajador.tipoContrato,
            fechaInicio: this.detalleTrabajador.fechaInicio,
            fechaFin: this.detalleTrabajador.fechaFin,
            areaTrabajo: this.detalleTrabajador.areaTrabajo,
            salario: this.detalleTrabajador.salario,
            barrio: this.detalleTrabajador.barrio,
            ciudad: this.detalleTrabajador.city,
            telefono: this.detalleTrabajador.telefono,
            email: this.detalleTrabajador.email,
            rol: this.detalleTrabajador.rol,
            anotacion: this.detalleTrabajador.anotacion,
          })
          .then((response) => {
            this.traerTrabajador();
            this.dialog = false;
            console.log(response);
            this.$store.dispatch("setDatos", response.data.item);
            this.$router.push("/AgregarTrabajadores");
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
    this.traerTrabajador();
    this.traerAreaTrabajo();
    this.traerDepartamentos();
  },
};
</script>