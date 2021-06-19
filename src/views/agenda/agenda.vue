<template>
<v-container >
        <Navbar :usr_name="user.usr_name" :usr_rol="user.usr_rol"> </Navbar>
<calendario></calendario>
</v-container>

</template>

<script>
import Navbar from "@/components/Navbar.vue";
import calendario from "@/components/Calendario.vue";

export default {
    data: () => ({

    }),
    methods: {},
    components: {
        Navbar,
        calendario
    },
        created: async function () {
        //cuando se carga la vista se cargan los datos del usuario que ingresó
        this.user = JSON.parse(sessionStorage.getItem("session"));
        if (this.user == null) {
            this.$router.push("/");
            //se redirecciona al Home si el objeto storage noe xistwe
        } else {
            try {
                //console.log(this.user);
                //const res = await this.axios.post("/clientes/mis-clientes", this.user);
                //console.log(res.data);
               // this.ClientesListar = res.data;
            } catch (error) {
                console.log(error);
            }
        }
    },
}
</script>
