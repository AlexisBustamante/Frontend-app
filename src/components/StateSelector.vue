<template>
<v-container >

<v-card dark>

<v-card-text>
<v-autocomplete
    label="donde vives?"
    :items="states"
    hint="enter the first three letters..."
    hide-no-data
    clearable
    multiple
    small-chips
    deletable-chips
    >
    </v-autocomplete>
</v-card-text>

</v-card>

</v-container>
</template>

<script>
export default {
    name:'StateSelector',
    data:()=>({
        states:[
            'Cerro navia',
            'quinta normal',
            'maipu',
            'renca',
            'san bernardo'

        ]
    })

}
</script>