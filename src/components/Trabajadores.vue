<template>
  <v-app id="inspire">
    <v-container fluid >
      <v-row class="justify-center" align="center" style="height:90vh">
        <v-col cols="12" sm="6" md="4" lg="3" xl="2">
          <v-card class="pt-0 light-green-accent-4" color="#41B44C"  max-width="340" outlined>
            <div>
              <h1 class="text-left mb-10">
                ASOCIADOS
                <font-awesome-icon icon="fa-solid fa-sliders" />
              </h1>
            </div>
            <v-spacer></v-spacer>
            <v-container>
              <v-row class="justify-end mt-1">
                <v-col cols="3">
                  <v-btn
                    color="#41B44C"
                    class="black"
                    outlined
                    rounded
                    text
                    to="/agregarTrabajadores"
                  >
                    <v-icon>mdi-login</v-icon>
                  </v-btn>
                </v-col>
              </v-row>
            </v-container>
          </v-card>
        </v-col>

        <v-col cols="12" sm="6" md="4" lg="3" xl="2">
          <v-card class="mx-auto" color="#41B44C" max-width="344" outlined>
            <div>
              <h1 class="text-left mb-10">
                DIRECTOS
                <font-awesome-icon icon="fa-solid fa-plug-circle-bolt" />
              </h1>
            </div>
            <v-spacer></v-spacer>
            <v-container>
              <v-row class="justify-end mt-1">
                <v-col cols="3">
                  <v-btn
                    color="#41B44C"
                    class="white"
                    outlined
                    rounded
                    text
                    to="/AgregarDirecto"
                  >
                    <v-icon>mdi-login</v-icon></v-btn
                  >
                </v-col>
              </v-row>
            </v-container>
          </v-card>
        </v-col>

        <v-col cols="12" sm="6" md="4" lg="3" xl="2">
          <v-card class="mx-auto" color="#41B44C" max-width="344" outlined>
            <div>
              <h1 class="text-left mb-10">
                TEMPORALES
                <v-list-item-icon>
                  <font-awesome-icon icon="fa-solid fa-handshake" />
                </v-list-item-icon>
              </h1>
            </div>
            <v-spacer></v-spacer>
            <v-container>
              <v-row class="justify-end mt-0">
                <v-col cols="3">
                  <v-btn
                    color="#41B44C"
                    class="white"
                    outlined
                    rounded
                    text
                    to="/AgregarTemporales"
                  >
                    <v-icon>mdi-login</v-icon></v-btn
                  >
                </v-col>
              </v-row>
            </v-container>
          </v-card>
        </v-col>

        <v-col cols="12" sm="6" md="4" lg="3" xl="2">
          <v-card class="mx-auto" color="#41B44C" max-width="344" outlined>
            <div>
              <h1 class="text-left mb-10">
                Bitacora <font-awesome-icon icon="fa-solid fa-list" />
              </h1>
            </div>
            <v-spacer></v-spacer>
            <v-container>
              <v-row class="justify-end mt-0">
                <v-col cols="3">
                  <v-btn
                    color="#41B44C"
                    class="white"
                    outlined
                    rounded
                    text
                    to="Bitacora"
                  >
                    <v-icon>mdi-login</v-icon>
                  </v-btn>
                </v-col>
              </v-row>
            </v-container>
          </v-card>
        </v-col>

         

        <v-col cols="12" sm="6" md="4" lg="3" xl="2">
          <v-card class="mx-auto " color="#41B44C" max-width="344" outlined>
            <div>
              <h3 class="text-left mb-10">Cantidad de Trabajadores</h3>
            </div>
            <v-spacer></v-spacer>
            <v-container>
              <v-row class="justify-end mt-0">
                <v-col cols="3">
                  <v-btn
                    color="#41B44C"
                    class="black"
                    outlined
                    rounded
                    text
                    to="/setup"
                  >
                    <v-icon>mdi-login</v-icon></v-btn
                  >
                </v-col>
              </v-row>
            </v-container>
          </v-card>
        </v-col>

        <v-col cols="12" sm="6" md="4" lg="3" xl="2">
          <v-card class="mx-auto " color="#41B44C" max-width="344" outlined>
            <div>
              <h3 class="text-left mb-10">usuarios</h3>
            </div>
            <v-spacer></v-spacer>
            <v-container>
              <v-row class="justify-end mt-0">
                <v-col cols="3">
                  <v-btn
                    color="#41B44C"
                    class="black"
                    outlined
                    rounded
                    text
                    to="/verusuario"
                  >
                    <v-icon>mdi-login</v-icon></v-btn
                  >
                </v-col>
              </v-row>
            </v-container>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>
<script>
export default {
  name: "pagesTrabajadores",
  data: () => ({}),
};
</script>
<style scoped>
.boton {
  position: relative;
}

.texto {
  position: absolute;
  top: -30px;
  font-size: 12px;
  background: rgb(220, 216, 216);
  color: black;
  padding: 8px;
  visibility: hidden;
  transition: hover 0.6s ease;
  border-radius: 10px;
}

.boton:hover .texto {
  visibility: visible; 
}
</style>