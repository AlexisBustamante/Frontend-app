<template>
  <v-container >
     <v-col cols="12">
      <v-row justify="center">
        <v-col md="6" sm="6">
          <v-card
            class="ma-4"
            justify="center"
            elevation="24"
            max-width="450"
            dark
          >
            <v-row justify="center">
              <v-avatar size="150 " class="ma-10">
                <v-img :src="require('../assets/Snake.svg')" contain />
              </v-avatar>
            </v-row>

            <v-card-title></v-card-title>

            <v-card-text>
              <v-form class="ma-3" @submit.prevent="signIn()" ref="signInForm">
                <v-text-field
                  label="E-mail"
                  prepend-icon="mdi-email"
                  :rules="emailRules"
                  v-model="user.email"
                ></v-text-field>

                <v-text-field
                  label="Contraseña"
                  prepend-icon="mdi-lock"
                  type="password"
                  :rules="passwordRules"
                  v-model="user.password"
                ></v-text-field>

                <v-btn block class="success mt-3" type="submit">Sign In</v-btn>
              </v-form>
            </v-card-text>

            <v-alert text v-model="alert.show" :type="alert.type" dismissible>
              {{ alert.message }}
            </v-alert>
          </v-card>
        </v-col>
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
// @ is an alias to /src

export default {
  data: () => ({
    alert: {
      show: false,
      message: "Error",
      type: "error",
    },
    nameRules: [
      (value) => !!value || "Campo Obligatorio",
      (value) =>
        (value && value.length >= 5) ||
        "El nombre debe contener más de 5 carácteres",
    ],
    emailRules: [
      (value) => !!value || "Campo obligatorio",
      (value) => /.+@.+\..+/.test(value) || "Ingrese E-mail válido", ///expresion para validar un email
    ],
    passwordRules: [
      (value) => !!value || "Campo obligatorio",
      (value) =>
        (value && value.length >= 6) ||
        "El password debe contener más de 6 carácteres",
    ],
    user: { name: "", email: "", password: "" },
    suForm: true,
  }),

  methods: {
    async signUp() {
      //$refs . Nombre del Formulario.
      let valid = this.$refs.signupForm.validate();
      //console.log(valid);

      if (valid) {
        try {
          //se le pasa el body this . user del formulario
          const res = await this.axios.post("/signup", this.user);

          this.$refs.signupForm.reset(); //todos lso cmapso en blanco ene l formulario
          this.suForm = false;
          //  console. log(res);

          this.alert = {
            show: true,
            type: "success",
            message: res.data.message,
          };
        } catch (error) {
          this.alert = {
            show: true,
            type: "error",
            message: error.response.data.message,
          };
        }
      }
    },

    async signIn() {
      let valid = this.$refs.signInForm.validate();
      //console.log(valid);

      if (valid) {
        try {
          //se le pasa el objeto ocn los parametros.
          const res = await this.axios.post("/signIn", this.user);
          
          //este log se ve ene el navegador, ya que es frontend.
         // console.log(res.data);

          if (res.data.Notfound) {
            this.alert = {
              show: true,
              message: res.data.message,
              type: "error",
            };
          } else {
            //si lopgra registrarse se añade a la ssesion storage el objeto usuario.
            // console.log(res.data.name);
            sessionStorage.setItem("session", JSON.stringify(res.data));
            this.$router.push("/profile");
            
            //se redirecciona a /profile
          }
        } catch (error) {
          this.alert = {
            show: true,
            type: "error",
            message: error.response.data.message,
          };
        }
      }
    },
  }, //methods
};
</script>

<style>

</style>
