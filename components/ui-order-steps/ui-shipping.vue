<template>
    <v-container>
            <v-row>
            <v-col cols="12">
                    <div class="d-flex align-center rounded-field">
                        <h6 class="mr-4 text-subtitle-2" >Enviar a</h6>
                        <v-text-field
                            v-model="shipAddress"
                            label="Dirección"
                            solo
                            dense
                            hide-details
                            clearable
                            flat
                        >
                        </v-text-field>
                    </div>
            </v-col>
            <v-col cols="12">
                    <h2 class="section-title py-2">Forma de envío</h2>
                    <v-form v-model="isFormValid">
                    
                    <v-radio-group
                        v-model="shipOptions"
                        class="mt-0 pt-0"
                    >
                        <div class="d-flex align-center justify-space-between rounded-field">
                            <v-radio
                                label="Envío estándar"
                                value="Gratis"
                            >
                            </v-radio>
                            <strong>Gratis</strong>
                        </div>
                
                        <div class="d-flex align-center justify-space-between rounded-field">
                            <v-radio
                                label="Envío rápido"
                                value="Fast"
                            >
                            </v-radio>
                            <strong>$ 100.00</strong>
                        </div>
                    </v-radio-group>
                </v-form>
            </v-col>
            <v-col cols="6">
                    <a 
                        class=""
                        @click="$nuxt.$emit('changeStep', {
                            step: 2,
                            fullName: '',
                            fullAddress: '',
                            shipType: ''
                    })"    
                    >
                        Regresar a Detalles
                    </a>
            </v-col>
            <v-col cols="6">
                <v-btn
                    color="primary"
                    height="48"
                    elevation="0"
                    block
                    :disabled="!shipOptions"
                    @click="$nuxt.$emit('changeStep', {
                        step: 4,
                        fullName: getName(),
                        fullAddress: getAddress(),
                        shipType: getShip()
                    })"       
                >
                    Ir al pago
                </v-btn>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
export default {
    name: 'uiShipping',
    data: () => ({
        shipDirection: '',
        isFormValid: false,
        shipOptions: false
    }),
    props: {
        name: String,
        shipAddress: String
    },
    methods: {
        getName () {
            return this.name
        },
        getAddress () {
            return this.shipAddress
        },
        getShip () {
            return this.shipOptions
        }
    },
}
</script>

<style>
.rounded-field{
    margin: 1rem 0;
    padding: 1rem;
    border-radius: 0.25rem;
    border: 1px solid lightgray;
}

.rounded-field:hover{
    border: 1px solid #56B280;
}
</style>