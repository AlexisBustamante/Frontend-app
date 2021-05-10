<template>
  <v-container>
    <Navbar :usr_name="user.usr_name" :usr_rol="user.usr_rol"> </Navbar>

    <v-alert text v-model="alert.show" :type="alert.type" dismissible>
      {{ alert.message }}
    </v-alert>

    <v-dialog max-width="650" v-model="add">
      <v-card dark>
        <v-card-title>Crear Cliente Nuevo </v-card-title>

        <v-card-text>
          <v-form ref="addClienteForm" @submit.prevent="addCliente()">
            <v-row>
              <v-col>
                <v-text-field
                  prepend-icon="mdi-account-box-multiple"
                  label="Nombres"
                  :rules="[(v) => !!v || 'Nombre es requerido']"
                  v-model="clienteToAdd.c_nombre"
                ></v-text-field>
              </v-col>
              <v-col>
                <v-text-field
                  label="Apellidos"
                  :rules="[(v) => !!v || 'Apellidos es requerido']"
                  v-model="clienteToAdd.c_apellidos"
                ></v-text-field>
              </v-col>
            </v-row>

            <v-row>
              <v-col>
                <v-text-field
                  prepend-icon="mdi-card-account-details-outline"
                  label="Direccion"
                  :rules="[(v) => !!v || 'Direccion es requerido']"
                  v-model="clienteToAdd.c_direccion"
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-text-field
                  prepend-icon="mdi-phone-dial"
                  label="Telefono "
                  v-model="clienteToAdd.c_telefono"
                ></v-text-field>
              </v-col>
              <v-col>
                <v-text-field
                  prepend-icon="mdi-email"
                  label="E-mail"
                  :rules="emailRules"
                  v-model="clienteToAdd.c_email"
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-btn block class="primary mt-3" type="submit">Agregar</v-btn>
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
      </v-card>
    </v-dialog>

    <v-card elevation="24" dark>
      <v-card-title>
        Clientes
        <v-spacer></v-spacer>
        <v-btn class="success mt-3" @click="add = true">Crear Cliente</v-btn>
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Buscar"
          single-line
          hide-details
        >
        </v-text-field>
      </v-card-title>
      <v-data-table :headers="headers" :items="ClientesListar" :search="search">
        <template v-slot:item.actions="{ item }">
          <v-icon
            dark
            color="primary"
            class="mr-2"
            @click="EditarCliente(item.c_id)"
          >
            mdi-pencil
          </v-icon>

          <v-icon
            dark
            color="red"
            class="mr-2"
            @click="EliminarCliente(item.c_id)"
          >
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
    </v-card>
  </v-container>
</template>


<script>
import Navbar from "@/components/Navbar.vue";
export default {
  data: () => ({
    load: false,
    add: false,
    search: "",
    clienteToAdd: {
      usr_id: 0,
    },
    emailRules: [
      (value) => !!value || "Campo obligatorio",
      (value) => /.+@.+\..+/.test(value) || "Ingrese E-mail válido", ///expresion para validar un email
    ],
    headers: [
      { text: "Nombres", value: "c_nombre" },
      { text: "Apellidos", value: "c_apellidos" },
      { text: "Telefono", value: "c_telefono" },
      { text: "E-mail", value: "c_email" },
      { text: "Dirección", value: "c_direccion" },
      { text: "Actions", value: "actions", sortable: false },
    ],
    itemsPerPageArray: [4, 8, 12],
    filter: {},
    sortDesc: false,
    page: 1,
    itemsPerPage: 4,
    sortBy: "c_id",
    keys: ["c_id"],
    user: {},
    ClientesListar: [],
    alert: {
      show: false,
      message: "Error",
      type: "error",
    },
  }),
  created: async function () {
    //cuando se carga la vista se cargan los datos del usuario que ingresó
    this.user = JSON.parse(sessionStorage.getItem("session"));
    if (this.user == null) {
      this.$router.push("/");
      //se redirecciona al Home si el objeto storage noe xistwe
    } else {
      try {
        //console.log(this.user);
        const res = await this.axios.post("/clientes/mis-clientes", this.user);
        //console.log(res.data);
        this.ClientesListar = res.data;
      } catch (error) {
        console.log(error);
      }
    }
  },

  methods: {

    async EliminarCliente(c_id) {

      try {
        const res = await this.axios.delete(`/clientes/eliminar/${c_id}`);

        
      } catch (error) {

        this.add = false;
        this.alert.show = true;
        this.alert.message = error.response.data.message;
        this.alert.type = "error";
      }
    },

    async EditarCliente(c_id) {
      console.log(c_id);
    },

    async addCliente() {
      //console.log(this.clienteToAdd);

      let valid = this.$refs.addClienteForm.validate();

      if (valid) {
        this.clienteToAdd.usr_id = this.user.id_usr;

        try {
          const res = await this.axios.post(
            "/clientes/nuevo-cliente",
            this.clienteToAdd
          );

          console.log(res);
          this.ClientesListar.push(res.data.cliente);
          this.ClientesListar.reverse();

          this.$refs.addClienteForm.reset();
          this.add = false;

          this.alert.show = true;
          this.alert.message = res.data.message;
          this.alert.type = "success";
        } catch (error) {
          // console.log(error);
          this.add = false;
          this.alert.show = true;
          this.alert.message = error.response.data.message;
          this.alert.type = "error";
        }
      }
    },
  },
  components: {
    Navbar,
  },
};
</script>
