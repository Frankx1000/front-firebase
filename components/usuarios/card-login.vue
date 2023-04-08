<template>
<v-card shaped elevation="5" width="500" color="#64B5F6">
    <v-card-tittle class="text-center">
       Inicio de Cesion
    </v-card-tittle>
    <v-card-text>
        <v-form ref="frmLogin">
            <v-text-field v-model="email" label="Email"  placeholder="Escribe tu correo" 
            :rules="validateEmail" />
            <v-text-field v-model="password" label="Password"  placeholder="Escribe tu password" 
            type="password" 
            :rules="validatePassword" />
        </v-form>
    </v-card-text>
    <v-card-actions>
<v-btn block class="colorBtn" @click="ingresarSistema">
            <v-icon dense style="padding-right: 20px;">
                mdi-account-key
            </v-icon>
            Ingresar
        </v-btn>    
    </v-card-actions>
    <v-dialog v-model="dialog" width="200" transition="dialog-bottom-transition">
<div style="width: 100px;height: 100px;background-color: red;">
<span style="font-size: 20px; font-weight: 800;color: white;">
{{ mensaje }}
</span>
</div>
    </v-dialog>

</v-card>
</template>

<script>
export default {
data () {
    return {
        email: '',
        password: '',
        emailRules: [ 
        v => !v || /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
      ],
      validatePassword: [
        v => !v || v.length >= 6 || 'Password must have min 6 chars'
      ],
      dialog: false,
      mensaje: ''
    }
},
methods: {
        async ingresarSistema () {
            if(this.email.length === 0 && this.password.length === 0) {
                alert('Algo anda Mal')
                return
            }
            if(this.$refs.frmLogin.validate()) {
                const sendData = {
                    email: this.email,
                    password: this.password
                }
                await this.$auth.loginWith('local', {
                    data: sendData
                }).then((res) => {
                    if(res.data.error === null) {
                        this.$router.push('/dashboard')
                    }
                }).catch((error) => {
                    this.mensaje = 'Hubo un error'
                    this.dialog = true
                    setTimeout(() => {
                     this.dialog = false
                    }, 2000)
                    console.log(error)
                })
            } else {
                alert('Algo anda Mal')
            }
        }
    }
  }
</script>

<style scoped>

.colorBtn {
    background-color: aqua!important;
}

.v-dialog__conatiner {
    display: flex;
}
</style>