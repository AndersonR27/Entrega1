<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-card flat>
        <v-card-actions>
          <h1>Listado de usuarios</h1>
          <v-spacer></v-spacer>
          <v-btn color="success" class="text-none" to="/users/creacion"
            >Crear usuario</v-btn
          >
        </v-card-actions>
        <v-card-text>
          <v-data-table
            :headers="headers"
            :items="users"
            :items-per-page="5"
            class="elevation-1"
          >
            <template v-slot:[`item.actions`]="{ item }">
              <v-icon class="mr-2" @click="editItem(item)">
                mdi-pencil
              </v-icon>
              <v-icon @click="deleteItem(item)">
                mdi-delete
              </v-icon>
            </template>
          </v-data-table>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
const url_api = "http://localhost:3001/users/";

export default {
  beforeMount() {
    this.getUsers();
  },
  data() {
    return {
      headers: [
        {
          text: "Identificación",
          align: "start",
          value: "id",
        },
        { text: "Nombres", value: "name" },
        { text: "Apellidos", value: "apellido" },
        { text: "Dirección", value: "direccion" },
        { text: "Contacto", value: "contacto" },       
        { text: "Correo", value: "email" },
        { text: "Acción", value: "actions" },
      ],
      users: [],
    };
  },
  methods: {
    /**
     * Enviar una solicitud (Request) en un método get
     * Para consultar todos los usuarios
     */
    async getUsers() {
      try {
        let response = await this.$axios.get(url_api);
        this.users = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    /**
     * Enviar la historia a edición
     * /users/id
     */
    editItem(item) {
      let url = `/users/${item.id}`;
      this.$router.push(url);
    },
    /**
     * Eliminar una historia
     */
    deleteItem(item) {
      this.$swal
        .fire({
          type: "warning",
          title: "¿Está seguro de eliminar el item?",
          text: "Al borrar el item no se podrá recuperar.",
          allowEscapeKey: false,
          allowOutsideClick: false,
          showCancelButton: true,
        })
        .then(async (result) => {
          if (result.value) {
            try {
              let url = url_api + item.id;
              await this.$axios.delete(url);
              this.$swal.fire({
                type: "success",
                title: "Operación exitosa.",
                text: "El item se eliminó correctamente.",
              });
              this.getUsers();
            } catch (error) {
              this.$swal.fire({
                type: "error",
                title: "Ha ocurrido un problema al eliminar",
                text: error.toString(),
              });
            }
          }
        });
    },
  },
};
</script>
