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
                <div class="d-flex align-center rounded-field">
                    <h6 class="mr-4 text-subtitle-2" >Tipo de envío</h6>
                    <v-text-field
                        v-model="shippingType"
                        label="Tipo de envío"
                        solo
                        dense
                        hide-details
                        clearable
                        flat
                    >
                    </v-text-field>
                </div>
            </v-col>
            <v-col cols="12" >
                <h2 class="section-title py-6">Método de pago</h2>
                <div style="" class="outlined-card">
                    <div class="d-flex px-7 py-4 outlined-card-header">
                        <v-icon left color="primary">mdi-credit-card</v-icon>
                        <h3 class="text-left section-title">Tarjeta de crédito</h3>
                    </div>
                    <v-form v-model="paymentForm">
                        <v-row class="mx-0 my-0 px-4 py-4">
                            <v-col cols="12">
                                <v-text-field
                                    v-model="cardNumber"
                                    type="number"
                                    label="Número de tarjeta"
                                    outlined
                                    hide-details="auto"
                                    hide-spin-buttons
                                    clearable
                                    :rules="[rules.required, rules.min_num]"
                                ></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field
                                    v-model="cardTitular"
                                    label="Nombre del titular"
                                    outlined
                                    hide-details
                                    clearable
                                    :rules="[rules.required]"
                                ></v-text-field>
                            </v-col>
                            <v-col cols="6">
                                <v-menu
                                    v-model="dateMenu"
                                    :close-on-content-click="false"
                                    :nudge-right="40"
                                    transition="scale-transition"
                                    offset-y
                                    min-width="auto"
                                >
                                    <template v-slot:activator="{ on, attrs }">
                                        <v-text-field
                                            v-model="computedDateFormatted"
                                            label="Fecha de expiración"
                                            hint="AA/MM"
                                            prepend-inner-icon="mdi-calendar"
                                            readonly
                                            outlined
                                            hide-details="auto"
                                            v-bind="attrs"
                                            v-on="on"
                                        ></v-text-field>
                                    </template>
                                    <v-date-picker
                                        v-model="cardExpDate"
                                        @input="dateMenu = false"
                                    ></v-date-picker>
                                </v-menu>
                                <!--
                                <v-text-field
                                    v-model="cardExpDate"
                                    label="Expiración (MM/YY)"
                                    outlined
                                    hide-details
                                    clearable
                                    :rules="[rules.required]"
                                ></v-text-field>
                                -->
                            </v-col>
                            <v-col cols="6">
                                <v-text-field
                                    type="number"
                                    v-model="cardCvv"
                                    label="CVV"
                                    outlined
                                    hide-details="auto"
                                    hide-spin-buttons
                                    clearable
                                    :rules="[rules.required, rules.valid_cvv]"
                                ></v-text-field>
                            </v-col>
                        </v-row>
                    </v-form>
                </div>
            </v-col>
            <v-col cols="6" class="pt-8">
                <a class="" @click="$nuxt.$emit('changeStep', {
                        step: 3,
                        fullName: getName(),
                        fullAddress: shipAddress,
                        shipType: ''
                    })"
                >
                    Regresar a envío
                </a>
            </v-col>
            <v-col cols="6" class="pt-8">
                <v-btn
                    color="primary"
                    height="48"
                    elevation="0"
                    block
                    :disabled="!paymentForm"
                    @click="$nuxt.$emit('changeStep', {
                        step: 5,
                        fullName: getName(),
                        fullAddress: shipAddress,
                        shipType: shippingType
                    })"   
                >
                    Finalizar
                </v-btn>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
export default {
    name: 'uiPayment',
    data: vm => ({
        cardExpDate: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
        paymentForm: false,
        dateMenu: false,
        rules: {
            required: value => !!value || 'Campo requerido',
            min_num: v => (v && (v.length > 15 && v.length < 17)) || 'Deben ser al menos 16 dígitos',
            valid_cvv: v => (v && (v.length > 2 && v.length < 4)) || 'Deben ser 3 dígitos'
        },
    }),
    computed: {
        computedDateFormatted () {
            return this.formatDate(this.cardExpDate)
      },
    },
    props: {
        name: String,
        shipAddress: String,
        shippingType: String
    },
    methods: {
        getName () {
            return this.name
        },
        formatDate (date) {
            if (!date) return null
            const [year, month, day] = date.split('-')
            return `${year}/${month}`
      },
    }
}
</script>

<style>
.outlined-card{
    border: 1px solid lightgray;
    border-radius: 1.25rem;
}

.outlined-card-header{
    border-radius: 1.25rem 1.25rem 0 0;
    color: #56B280;
    background: linear-gradient(to right, rgba(86, 178, 128, 0.2), rgba(86, 178, 128, 0.4));
}
</style>