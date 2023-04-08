<template>
    <v-row class="principal" style="margin: 10px; padding: 10px;">
        <v-btn block color="green darkeen-4" @click="dialogC">
            Agregar Usuario
        </v-btn>
        <v-col cols="12">
            <v-row class="principal">
                Usuarios del Sistema
            </v-row>   
            <v-row class="principal">
                <v-data-table
                    :headers="headers"
                    :items="usuarios"
                    style="width: 100%;"
                >
            <template #[`item.acciones`]="{ item }">
                <v-row>
                    <v-col cols="6">
                    <v-btn icon color="orange" @click="dialogU(item)">
                        <v-icon>mdi-human-edit</v-icon>
                    </v-btn>
                </v-col>
                <v-col cols="6">
                    <v-btn icon color="red" @click="dialogDelete(item)">
                        <v-icon>mdi-eraser</v-icon>
                    </v-btn>
                </v-col>
                </v-row>
            </template>
            </v-data-table>
            </v-row> 
        </v-col>
        <v-dialog
      v-model="dialogBorrado"
      max-width="290"
      persistent
      >
      <v-card>
        <v-card-title class="text-h5">
          Borrado de Usuario
        </v-card-title>
        <v-card-text>
          ¿Estas seguro que deseas borrar usuario?
        </v-card-text>

        <v-card-actions>
          <v-spacer />

          <v-btn
            color="green darken-1"
            text
            @click="dialogBorrado = false"
          >
            Cancelar
          </v-btn>

          <v-btn
            color="red darken-4"
            text
            @click="borrar()"
          >
            Aceptar
          </v-btn>
        </v-card-actions>
      </v-card>
      </v-dialog>
      <v-dialog
      v-model="dialogUpdate"
      max-width="400"
      persistent
     >      
    <v-card>
        <v-card-title>Actualizacion de Datos</v-card-title>
        <v-card-text>
            <v-form ref="frmUpdate">
                Nombre:
                <v-text-field
                v-model="user.name"
                placeholder="Nombre"
                :rules="reglaNombre"               
                />
                Apellido:
                <v-text-field
                v-model="user.lastname"
                placeholder="Apellido"               
                />
                Numero:
                <v-text-field
                v-model="user.number"
                placeholder="Numero"               
                />
                Password:
                <v-text-field
                v-model="password"
                placeholder="Password"
                type="password"      
                :rules="reglaPassword"           
                />
            </v-form>
        </v-card-text>
        <v-card-actions>
            <v-btn color="warning" @click="dialogUpdate=false">
                Cancelar
            </v-btn>
            <v-btn color="green" @click="update">
                Actualizar
            </v-btn>
        </v-card-actions>
    </v-card>
    </v-dialog>

    <v-dialog
      v-model="dialogCrear"
      max-width="400"
      persistent
     >      
    <v-card>
        <v-card-title>Crear Usuario</v-card-title>
        <v-card-text>
            <v-form ref="frmCrear">
                Nombre:
                <v-text-field
                v-model="user.name"
                placeholder="Nombre"
                :rules="reglaNombre"               
                />
                Apellido:
                <v-text-field
                v-model="user.lastname"
                placeholder="A. Paterno"               
                />
                Telefono:
                <v-text-field
                v-model="user.number"
                placeholder="A. Materno"               
                />
                Correo:
                <v-text-field
                v-model="user.email"
                placeholder="Correo"               
                />
                Password:
                <v-text-field
                v-model="password"
                placeholder="Password"
                type="password"      
                :rules="reglaPassword"           
                />
            </v-form>
        </v-card-text>
        <v-card-actions>
            <v-btn color="warning" @click="dialogCrear=false">
                Cancelar
            </v-btn>
            <v-btn color="green" @click="crear">
                Registrar
            </v-btn>
        </v-card-actions>
    </v-card>
    </v-dialog>

    </v-row>
</template>

<script>
export default {
    data () {
        return {
            usuarios: [],
            headers: [
            {
                text: 'Nombre',
                sortable: true,
                value: 'name'
            },
            {
                text: 'Apellido',
                sortable: true,
                value: 'lastname'
            },
            {
                text: 'Correo',
                sortable: true,
                value: 'email'
            },
            {
                text: 'Numero',
                sortable: true,
                value: 'number'
            },
            {
                text: 'Acciones',
                sortable: true,
                value: 'acciones'
            }
        ],
        dialogBorrado: false,
        idBorrar: '',
        dialogUpdate: false,
        dialogCrear: false,
        user: {} ,
        password: '',
        reglaNombre: [
                v => !v || v.length >= 3 || 'Name must have min 3 chars'
            ],
        reglaPassword: [
                v => !v || v.length >= 6 || 'Password must have min 6 chars'
            ],
        }
    },
    created () {
        this.loadUsers()
    },
    methods: {
        async loadUsers() {
            const config = {
                headers: {
                'Content-Type': 'application/json;charset=UTF-8',
                'Access-Control-Allow-Origin': '*'
                }
            }
        await this.$axios.get('user/traertodo', config)
        .then((res) => {
            this.usuarios = res.data.data
            console.log(res)
        })
        .catch((error) => {
            console.log(error)
        })
      },
      dialogDelete (item) {
        this.idBorrar = item.email
        this.dialogBorrado = true
      },
      async borrar () {
        const config = {
                headers: {
                'Content-Type': 'application/json;charset=UTF-8',
                'Access-Control-Allow-Origin': '*'
                }
            }
            const sendData = {
                email: this.idBorrar
            }
            await this.$axios.post('user/eliminar', sendData, config)
            .then((res) => {
            if(res.data.error === null) {
                this.dialogBorrado = false
                this.loadUsers()
            } else {
                alert(res.data.data)
            }
            })
            .catch((e) => {
                console.log(e)
            })
      },
      dialogU (item) {
        this.user = item
        this.dialogUpdate = true
      },
      async update() {
        const config = {
                headers: {
                'Content-Type': 'application/json;charset=UTF-8',
                'Access-Control-Allow-Origin': '*'
                }        
           }
           const userUpdate = {
            email: this.user.email,
            name: this.user.name,
            lastname: this.user.lastname,
            number: this.user.number,
            password: this.password
           }
           await this.$axios.post('user/actualizar',
            userUpdate,
            config
            ).then((res) => {
                console.log(res)
                this.dialogUpdate = false
                this.loadUsers()
            }).catch((e) => {
                console.log(e)
            })
        },
        dialogC (item) {
        this.user = item
        this.dialogCrear = true
      },
      async crear() {
        const config = {
                headers: {
                'Content-Type': 'application/json;charset=UTF-8',
                'Access-Control-Allow-Origin': '*'
                }        
           }
           const userCrear = {
            name: this.user.name,
            lastname: this.user.lastname,
            number: this.user.number,
            email: this.user.email,
            password: this.password
           }
           await this.$axios.post('user/insertar',
            userCrear,
            config
            ).then((res) => {
                console.log(res)
                this.dialogCrear = false
                this.loadUsers()
            }).catch((e) => {
                console.log(e)
            })
        }
    }
 }
</script>

<style scoped>
.principal {
    width: 100%;
}
</style>