<template>
    <div id="Cliente">
        <br />
        <h2 >REGISTRO DE CLIENTES</h2>
        <br />
        
        <form>
            <div class = "formulario" >

                <div class="form-row" >
                    <div class="form-group col-md-5">
                        <label for="Phone1">Teléfono:</label>
                        <input type="number" class="form-control" id="Phone" name="Phone" value=""  placeholder="Teléfono"/>
                    </div>
                    <div class="form-group col-md-5">
                        <label for="name">Nombre:</label>
                        <input type="text" class="form-control" id="name" name="name" value="" placeholder="Nombre"/>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group col-md-5">
                        <label for="idCC">Cédula:</label>
                        <input type="text" class="form-control" id="idCC" name="idCC" value="" placeholder="Cédula"/>  
                    </div>
                    <div class="form-group col-md-5">
                        <label for="birth">Cumpleaños:</label>
                        <input type="text" class="form-control" id="birth" name="birth" value="" placeholder="AAAA-MM-DD"/>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group col-md-5">
                        <label for="adress">Dirección:</label>
                        <input type="text" class="form-control" id="adress" name="adress" value="" placeholder="Cra. 24 # ..."/>
                    </div>
                
                    <div class="form-group col-md-5">
                        <label for="zone">Barrio:</label>
                        <input type="text" class="form-control" id="zone" name="zone" value="" placeholder="Barrio"/>
                    
                    </div>
                </div>
            </div>
            <br>
            <div class="botones">
                <div style='text-align:center'>
                    <right>
                        <button type="button" class="btn btn-warning" v-on:click="myProvider">Lista</button>
                        <button type="button" class="btn btn-warning"  v-on:click="findCliente">Buscar</button>
                        <button type="button" class="btn btn-warning" v-on:click="makeCliente">Crear</button>
                        <!-- <button type="button" class="btn btn-warning" >Actualizar</button> -->
                        <button type="button" class="btn btn-warning" v-on:click="cleanCampos">Limpiar</button>
                        <button type="button" class="btn btn-warning" v-on:click="deleteCliente">Eliminar</button><br /><br />
                        

                    </right>
                </div>
            </div>
        
        </form>
        <br />
        
        <b-table sticky-header ref="table" id="my-table" striped hover :items="items"></b-table>
        
    </div>
</template>

<script>

import axios from "axios";
export default {
    name: "Cliente",

    data: function () {
        return {
            
            telefono: 0,
            nombre: "",
            direccion: "",
            barrio: "",
            cedula: "",
            cumpleanos: "",
            balance: 0,
            newCliente: {},
            items: []
            
            
        };
    },

    methods: {
        init: function () {
        if (this.$route.name != "cliente") {
            let username = input.Phone.getItem("current_username");
            this.$router.push({name: "cliente", params:{ username: 'username' }});
        }
        },

        findCliente: function () {
            this.telefono = document.getElementById("Phone").value
            let self = this
            axios.get("http://127.0.0.1:8000/cliente/consulta/" + this.telefono)
                .then((result) => {
                    self.telefono = result.data.telefono
                    self.nombre = result.data.nombre
                    self.direccion = result.data.direccion
                    self.barrio = result.data.barrio
                    self.cedula = result.data.cedula
                    self.cumpleanos = result.data.cumpleanos
                    confirm("Se encontró el cliente " + self.nombre);
                    document.getElementById("Phone").value = self.telefono;
                    document.getElementById("name").value = self.nombre;
                    document.getElementById("adress").value = self.direccion;
                    document.getElementById("zone").value = self.barrio;
                    document.getElementById("idCC").value = self.cedula;
                    document.getElementById("birth").value = self.cumpleanos;
                    
                })
                .catch((error) => {
                    alert("No se encontró el cliente");
                });

        },
        cleanCampos: function () {
            
            document.getElementById("Phone").value = ""
            document.getElementById("name").value = ""
            document.getElementById("adress").value = ""
            document.getElementById("zone").value = ""
            document.getElementById("idCC").value = ""
            document.getElementById("birth").value = ""
                                
        },
        makeCliente: function () {
            this.telefono = document.getElementById("Phone").value
            this.nombre = document.getElementById("name").value
            this.direccion = document.getElementById("adress").value
            this.barrio = document.getElementById("zone").value
            this.cedula = document.getElementById("idCC").value
            this.cumpleanos = document.getElementById("birth").value

            this.newCliente = {
                            "telefono": parseInt(this.telefono, 10),
                            "nombre": this.nombre,
                            "direccion": this.direccion,
                            "barrio": this.barrio,
                            "cedula": this.cedula,
                            "cumpleanos": this.cumpleanos
            }    
            let self = this            
            axios.post("http://127.0.0.1:8000/cliente/crear/", this.newCliente)
                .then((result) => {
                    window.confirm("Cliente Creado");
                    
                })
                .catch((error) => {
                    alert("ERROR Servidor");
                });
            
            this.myProvider()
            this.$refs.table.refresh()

        },
        myProvider: function () {
            console.log("Entro");
            let self = this
            
            axios.get("http://127.0.0.1:8000/cliente/lista/")
            .then((result) => {
                self.items = result.data
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })
        },
        deleteCliente: function () {
            this.telefono = document.getElementById("Phone").value
            this.cliente = {
                            "telefono": this.telefono
                            } 
            let telefono = this.cliente
            let self = this
            axios.delete("https://restaurante-back-g1.herokuapp.com/cliente/delete/", {data: telefono})
                .then((result) => {
                    
                    confirm("Se elimino de manera satisfactoria");
                    
                    
                })
                .catch((error) => {
                    alert("ERROR Servidor");
                });
            this.myProvider()
            this.$refs.table.refresh()

        },
        
        

        
        
    },   

};
</script>

<style>
#Cliente {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-content: center;
    justify-items: center; 
    margin-bottom: 0%;
}
#Cliente h2{
    width: 100%;
    text-align: center;
    margin-top: 1%;
    color:  #fffdfd;
}
#Cliente .formulario {
    color:  #fffdfd;
    align-content: center;
    display: block;
    margin-left: 15%;
    font-weight: bold;
    margin-bottom: 1%;
}

#Cliente button {
    color:  #181818;  
}
#Cliente .btn-warning{
    background-color: #f5a018;
}
#Cliente .botones {
    color:  #181818;
    width: 100%;
    text-align: center;
    margin-top: 0%;
}

.b-table{
    overflow:auto;
    margin-left: 0%;
    background-color: #fffdfd;
}
.b-table-sticky-header, .table-responsive, [class*=table-responsive-] {
    margin-bottom: 0rem;
    text-align: center;
    margin: 5%;
}

</style>
