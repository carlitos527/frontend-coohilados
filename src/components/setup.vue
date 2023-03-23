<template>
  <v-container class="light-green accent-2" fluid>

    <v-row>
      <v-col>
        <v-img
          src="http://1.bp.blogspot.com/-GAbYEVf6WrU/TzPM-nQbR_I/AAAAAAAAABA/9ElHB1Pp-2k/s1600/000.JPG"
        >

         <v-col class="green darken-2">
        
        <v-data-table class="yellow accent-1" :headers="headers" :items="setup">
          <template  v-slot:top>
            <v-toolbar class="green darken-2" flat>
              <v-toolbar-title>Trabajadores</v-toolbar-title>
              <v-divider class="green darken-2 mx-4" inset vertical></v-divider>
              <v-spacer></v-spacer>
            </v-toolbar>
          </template>
          
          <template v-slot:no-data>
            <h2>No hay datos</h2>
          </template>
        </v-data-table>
      </v-col>
        
    <v-row>
      
     
      <v-col></v-col>
    </v-row>
        
        </v-img>
      </v-col>
    </v-row>
  </v-container>
</template>  
<script>
import axios from "axios";
export default {
  name: "PagesSetup",
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: "Cantidad de Trabajadores Asociados",
        align: "start",

        value: "consecutivoTrabajador",
      },
      { text: "Cantidad de Trabajadores Temporales", value: "consecutivoTemporal" },
      { text: "Cantidad de Trabajadores Directos", value: "consecutivoDirecto" },
    ],

    setup: [],

    consecutivo: [],
    valid: true,
    consecutivoTrabajador: "",
    consecutivoTrabajadorRules: [
      (v) => !!v || "Consecutivo Trabajador es requerido",
      (v) =>
        (v && v.length <= 10) ||
        "El Consecutivo Trabajador debe tener menos de 10 caracteres",
    ],

    valid2: true,
    consecutivoTemporal: "",
    consecutivoTemporalRules: [
      (s) => !!s || "Consecutivo Temporal es requerido",
      (s) =>
        (s && s.length <= 10) ||
        "Consecutivo Temporal debe tener menos de 10 caracteres",
    ],

    valid3: true,
    consecutivoDirecto: "",
    consecutivoDirectoRules: [
      (s) => !!s || "Consecutivo Temporal es requerido",
      (s) =>
        (s && s.length <= 10) ||
        "Consecutivo Temporal debe tener menos de 10 caracteres",
    ],

  }),

  methods: {

    editItem(setup){
      this.dialog=true
      console.log(setup)


    },
    abrir() {
      this.dialog = true;
    },
   
    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },
    traerSetup() {
      axios
        .get("https://back-coohilados.vercel.app/api/setup")
        .then((response) => {
          this.setup = response.data.setcod;
          console.log(this.setup);
          console.log(response);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },

  created() {
    this.traerSetup();
  },
};
</script>