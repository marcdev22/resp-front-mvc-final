<template>
    <v-container fluid class="main-container">
        <v-card width="480" flat class=" py-6">
            <v-container>
                <v-card-text class="text-center py-2 ">
                    <h6 class="text-h5 py-2">
                        Editar cuenta
                    </h6>
                </v-card-text>
                <v-form>
                <div>
                    <h6 class="pb-6 text-h6">
                        Correo
                    </h6>
                    <v-text-field
                        v-model="userEmail"
                        label="Correo electrónico"
                        hint="example@email.com"
                        color="primary"
                        prepend-icon="mdi-email-outline"
                        :rules="[rules.required]"
                        outlined
                        clearable
                    >
                    </v-text-field>
                </div>
                <div class="py-4">
                    <h6 class="py-8 text-h6">
                        Contraseña
                    </h6>
                    <v-text-field
                        v-model="userPassword"
                        label="Contraseña"
                        color="primary"
                        prepend-icon="mdi-pencil"
                        :append-icon="showPass ? 'mdi-eye' : 'mdi-eye-off'"
                        :type="showPass ? 'text' : 'password'"
                        :rules="[rules.required, rules.min]"
                        @click:append="showPass = !showPass"
                        @click:prepend="passwordDialog = true"
                        outlined
                        clearable
                    >
                    </v-text-field>
                </div>
                </v-form>
                <v-card-actions>
                    <v-col cols="6">
                        <v-btn
                        height="48"
                        color="primary"
                        block
                        depressed
                        outlined
                        @click=""
                    >
                        <span>Cancelar</span>
                    </v-btn>
                    </v-col>
                    <v-col cols="6">
                        <v-btn
                        height="48"
                        color="primary"
                        block
                        depressed
                        @click=""
                    >
                        <span style="color: white;">Guardar</span>
                    </v-btn>
                    </v-col>
                </v-card-actions>
            </v-container>
            <!--Password Dialog-->
            <v-dialog
                v-model="passwordDialog"
                width="448"
                persistent
            >
                <v-card width="448"  flat>
                    <v-btn
                        icon
                        @click="passwordDialog=false"
                    >
                        <v-icon>mdi-window-close</v-icon>
                    </v-btn>
                     <v-container class="px-12">
                        <h2 class="text-center pb-8">Crea una nueva contraseña</h2>
                        <v-form v-model="newPassForm"> 
                            <v-text-field
                                v-model="newPassword"
                                value=""
                                label="Nueva contraseña"
                                color="primary"
                                :append-icon="showNewPass ? 'mdi-eye' : 'mdi-eye-off'"
                                :type="showNewPass ? 'text' : 'password'"
                                :rules="[rules.required, rules.min]"
                                outlined
                                clearable
                                counter
                                @click:append="showNewPass = !showNewPass"
                            >
                            </v-text-field>
                            <v-text-field
                                v-model="repeatPassword"
                                value=""
                                label="Repetir contraseña"
                                color="primary"
                                type="password"
                                :rules="[rules.required, passwordMatch]"
                                outlined
                                clearable
                            >
                            </v-text-field>
                        </v-form>
                        <v-card-actions>
                            <v-btn
                                color="primary"
                                height="48"
                                block
                                depressed
                                :disabled="!newPassForm"
                                @click=""
                            >
                                <span style="color: white;">Registrarme</span>
                            </v-btn>
                        </v-card-actions>

                    </v-container>
                </v-card>
            </v-dialog>
        </v-card>
    </v-container>
</template>

<script>
export default {
    layout:'default',
    data: () => ({
        showPass: false,
        showNewPass: false,
        passwordDialog: false,
        newPassForm: false,
        newPassword: "",
        repeatPassword: "",
        custom: true,
        rules: {
            required: value => !!value || 'Campo requerido',
            min: v => (v && v.length >= 8) || 'Al menos 8 caracteres',
        },
        
            
        
    }),
    computed: {
        progress () {
            return Math.min(100, this.value.length * 10)
        },
        color () {
            return ['error', 'warning', 'success'][Math.floor(this.progress / 40)]
        },
    },
    methods: {
        passwordMatch(value) {
            return (value === this.newPassword) || 'Las contraseñas no coinciden';
    },
    }
}
</script>

<style>
.main-container{
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #F2F2F2;

}
</style>