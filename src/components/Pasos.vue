<template>
<v-stepper v-model="currentStep">
    <v-stepper-header>
        <v-stepper-step :complete="currentStep>1" step="1">shipping</v-stepper-step>
        <v-divider></v-divider>
        <v-stepper-step :complete="currentStep>2" step="2">payment</v-stepper-step>
        <v-divider></v-divider>
        <v-stepper-step step="3">place order</v-stepper-step>
        <v-divider></v-divider>
    </v-stepper-header>
    <v-stepper-items>
        <v-stepper-content step="1">
            <v-card>
                <v-card-title primary-title>
                    Paso 1
                </v-card-title>
            
                <v-card-actions>
                    <v-btn flat color="primary"
                    @click="currentStep=2"
                    >Siguiente</v-btn>
                </v-card-actions>
            </v-card>
        </v-stepper-content>    
         <v-stepper-content step="2">
            <v-card>
                <v-card-title primary-title>
                    Paso 2
                </v-card-title>
            
                <v-card-actions>
                    <v-btn flat color="primary"
                    @click="currentStep=3"
                    >Siguiente</v-btn>
                </v-card-actions>
            </v-card>
        </v-stepper-content>    
           <v-stepper-content step="3">
            <v-card>
                <v-card-title primary-title>
                    Paso 3
                </v-card-title>
            
                <v-card-actions>
                    <v-btn flat color="primary"
                    @click="currentStep=1"
                    >Siguiente</v-btn>
                </v-card-actions>
            </v-card>
        </v-stepper-content>    
    </v-stepper-items>
  



</v-stepper>
</template>

<script>
export default {
    name: 'Checkout',
    data: () => ({
        currentStep: 1
    }),

};
</script>
