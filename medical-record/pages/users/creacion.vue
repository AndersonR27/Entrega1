<template>
  <div>
    <v-card flat>
      <v-card-actions>
        <h1>Creación de usuarios</h1>
        <v-spacer></v-spacer>
        <v-btn class="text-none" to="/users">Ver lista de usuarios</v-btn>
      </v-card-actions>

      <v-form ref="formUser" v-model="valid" lazy-validation>
        <v-text-field
          v-model="user.id"
          label="Identificación"
          :rules="rules.required"
          required
        ></v-text-field>

        <v-text-field
          v-model="user.name"
          :rules="rules.required"
          label="Nombres"
          required
        ></v-text-field>

        <v-text-field
          v-model="user.apellido"
          :rules="rules.required"
          label="Apellidos"
          required
        ></v-text-field>

        <v-text-field
          v-model="user.direccion"
          :rules="rules.required"
          label="Dirección"
          required
        ></v-text-field>

        <v-text-field
          v-model="user.contacto"
          :rules="rules.required"
          label="Contacto"
          required
        ></v-text-field>

        <v-text-field
          v-model="user.email"
          :rules="emailRules"
          label="Correo"
          required
        ></v-text-field>

        <v-text-field
          v-model="user.password"
          :rules="rules.required"
          label="Contraseña"
          type="password"
          required
        ></v-text-field>

        <v-select
          v-model="user.rol"
          :items="roles"
          label="Rol"
          item-value="id"
          item-text="nombre"
        ></v-select>

        <v-btn color="success" @click="saveUser()">Crear usuario</v-btn>
      </v-form>
    </v-card>
  </div>
</template>

<script>
const url_api = "http://localhost:3001/users/";
export default {
  data: () => ({
    valid: true,
    user: {},
    emailRules: [
      (v) => !!v || "El correo es obligatorio",
      (v) => /.+@.+\..+/.test(v) || "El correo no es valido",
    ],
    rules: {
      required: [(v) => !!v || "El campo es obligatorio"],
    },
    roles: [
      { id: 1, nombre: "Administrador" },
      { id: 2, nombre: "Médico" },
      { id: 3, nombre: "Paciente" },
      { id: 4, nombre: "Invitado" },
    ],
  }),
  beforeMount() {},

  methods: {
    /**
     * Enviar una solicitud (Request) en un método update
     * Para modificar el usuario
     */
    async saveUser() {
      if (this.$refs.formUser.validate()) {
        // Crear un nuevo objeto con la info del usuario
        let user = Object.assign({}, this.user);
        let response = await this.$axios.post(url_api, user);
        this.$swal.fire({
          type: "success",
          title: "Operación exitosa.",
          text: "El item se guardo correctamente.",
        });
      } else {
        this.$swal.fire({
          type: "warning",
          title: "Formulario incompleto.",
          text: "Hay campos que deben ser diligenciados.",
        });
      }
    },
  },
};
</script>
<style></style>
