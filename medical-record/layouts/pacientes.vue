<template>
  <v-app v-if="user">
    <v-app-bar fixed app dark color=#39F>
      <!-- <v-app-bar-nav-icon></v-app-bar-nav-icon> -->

      <v-toolbar-title>Bienvenido {{ user.name }}</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn class="text-none" small text to="/">
        <v-icon>mdi-exit-to-app</v-icon> Cerrar sesión
      </v-btn>
    </v-app-bar>
    <v-main>
      <nuxt />
    </v-main>
    <v-footer app>
      <v-spacer></v-spacer>
      
      <v-spacer></v-spacer>      
    </v-footer>
    <!-- Login -->
    <v-dialog v-model="dialogLogin" max-width="290" persistent>
      <v-card>
        <v-card-text> </v-card-text>
        <login />
        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="green darken-1" text @click="dialogLogin = false">
            Cancelar
          </v-btn>
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
  <v-app v-else>
    <v-main>
      <center>
        <h3>Por favor inicie sesión</h3>
        <v-btn color="green darken-1" text to="/login">
          Ir al login
        </v-btn>
      </center>
    </v-main>
  </v-app>
</template>

<script>
import login from "@/components/Login";
export default {
  components: {
    login,
  },
  beforeMount() {
    this.loadUser();
  },
  data() {
    return {
      year: new Date().getFullYear(),
      dialogLogin: false,
      user: null,
    };
  },
  methods: {
    loadUser() {
      let stringUser = localStorage.getItem("user-in");
      this.user = JSON.parse(stringUser);
      this.validRol(this.user);
    },
    validRol(user) {
      if (!user || user.rol != 3) {
        this.$router.push("/");
      }
    },
  },
};
</script>
<style></style>
