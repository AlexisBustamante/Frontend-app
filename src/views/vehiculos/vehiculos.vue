<template>
<v-container>
    <Navbar :usr_name="user.usr_name" :usr_rol="user.usr_rol"> </Navbar>

    <v-alert text v-model="alert.show" :type="alert.type" dismissible>
        {{ alert.message }}
    </v-alert>

<pasos></pasos>
<StateSelector></StateSelector>
<Calendar></Calendar>

<calendario></calendario>

</v-container>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import pasos from "@/components/Pasos.vue";
import StateSelector from "@/components/StateSelector.vue";
import Calendar from "@/components/Calendar.vue";

import calendario from "@/components/Calendario.vue";

export default {
    data: () => ({
        search: '',
        add: false,
        del: false,
        vehiculosListar: [],
        alert: {
            show: false,
            message: "Error",
            type: "error",
        },
        headers: [{
                text: "Propietario",
                value: "ve_propietario"
            },
            {
                text: "Patente",
                value: "ve_patente"
            },
            {
                text: "Año",
                value: "ve_año"
            },
            {
                text: "Modelo",
                value: "modeldesc"
            },
            {
                text: "Marca",
                value: "marcdesc"
            },
            {
                text: "Actions",
                value: "actions",
                sortable: false
            }
        ],

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
                const res = await this.axios.post("/vehiculos//mis-vehiculos", this.user);
                console.log(res.data);
                this.vehiculosListar = res.data;
            } catch (error) {
                console.log(error);
            }
        }
    },

    methods: {

    },
    components: {
        Navbar,
        pasos,
        StateSelector,
        Calendar,
        calendario
        
    },
};
</script>
