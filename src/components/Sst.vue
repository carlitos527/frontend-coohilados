<template>
  <v-container fluid>
    <v-row>
      <v-col>
        <v-template>
          <v-toolbar class="amber accent-2">
            <v-toolbar-title
              ><h1>TRABAJADORES COOHILADOS<transition> </transition></h1>
            </v-toolbar-title>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-text-field
              v-model="busqueda"
              append-icon="mdi-magnify"
              label="buscar"
              single-line
              hide-details
            >
            </v-text-field>
            <v-spacer></v-spacer>
          </v-toolbar>
        </v-template>

        <!--   informacion de todos los tranbajadores -->
        <template>
          <v-data-table
            :headers="headers"
            :items="directos"
            :search="busqueda"
            sort-by="nombre"
            class="elevation-1 amber lighten-3"
          >
            <template>
              <v-toolbar flat>
                <v-card>
                  <v-card-title> </v-card-title>
                  <v-card-text>
                    <v-container v-for="item in directos" :key="item._id">
                      <v-row>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field v-model="nombre" label="Nombre">{{
                            item.nombre
                          }}</v-text-field>
                        </v-col>
                        <v-col>
                          <v-text-field
                            v-model="tipoDocumento"
                            label="Tipo de Documento"
                            >{{ item.tipoDocumento }}
                          </v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field v-model="documento" label="Documento">{{
                            item.documento
                          }}</v-text-field>
                        </v-col>

                        <v-col cols="12" sm="6" md="4">
                          <v-text-field v-model="rol" label="Cargo">{{
                            item.rol
                          }}</v-text-field>
                        </v-col>

                        <v-col cols="12" sm="6" md="4">
                          <v-text-field v-model="estado" label="Estado">{{
                            item.estado
                          }}</v-text-field>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card-text>
                </v-card>
              </v-toolbar>
            </template>

            <template v-slot:[`item.fechaNacimiento`]="{ item }">
              <span>
                {{ fecha(item.fechaNacimiento) }}
              </span>
            </template>
            <template v-slot:[`item.fechaInicio`]="{ item }">
              <span>
                {{ fecha(item.fechaInicio) }}
              </span>
            </template>
            <template v-slot:[`item.fechaFin`]="{ item }">
              <span>
                {{ fecha(item.fechaFin) }}
              </span>
            </template>
          </v-data-table>
        </template>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "PagesSst",
  data() {
    return {};
  },
};
</script>