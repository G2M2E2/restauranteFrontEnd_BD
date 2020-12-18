<template>
    <div id="Venta">
        <br />
        <h2 >MÓDULO DE VENTAS</h2>
        <br />
        
        <form>
            <div class = "formulario" >

                <div class="form-row" >
                    <div class="form-group col-md-3">
                        <label for="idventa">Id Venta:</label>
                        <input type="number" class="form-control" id="idventa" name="idventa" value=""  placeholder="Id"/>
                    </div>
                    <div class="form-group col-md-3">
                        <label for="idprod">Id Producto:</label>
                        <input type="text" class="form-control" id="idprod" name="idprod" value=""  placeholder="Id_producto"/>
                    </div>
                    <div class="form-group col-md-3">
                        <label for="nomprod">Nombre:</label>
                        <input type="text" class="form-control" id="nomprod" name="nomprod" value="" placeholder="Nombre"/>
                    </div>
                    </div>
                <div class="form-row">
                    
                    <div class="form-group col-md-3">
                        <label for="precprod">Precio:</label>
                        <input type="number" class="form-control" id="precprod" name="precprod" value="" placeholder="Precio"/>  
                    </div>
                    <div class="form-group col-md-3">
                        <label for="cantprod">Cantidad:</label>
                        <input type="number" class="form-control" id="cantprod" name="cantprod" value="" placeholder="Cantidad"/>
                    </div>
                    <div class="form-group col-md-3">
                        <label for="subtotal">Subtotal:</label>
                        <input type="number" class="form-control" id="subtotal" name="subtotal" value="" placeholder="Subtotal"/>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group col-md-3">
                        <label for="fechaventa">Fecha:</label>
                        <input type="text" class="form-control" id="fechaventa" name="fechaventa" value="" placeholder="Fecha"/>
                    </div>
                    <div class="form-group col-md-3">
                        <label for="telefono">Teléfono:</label>
                        <input type="number" class="form-control" id="telefono" name="telefono" value="" placeholder="Teléfono"/>
                    </div>
                </div>
            </div>
            <br>
            <div class="botones">
                <div style='text-align:center'>
                    <right>
                        <button type="button" class="btn btn-warning" v-on:click="myProvider">Lista</button>
                    <!--<button type="button" class="btn btn-warning"  v-on:click="findVenta">Buscar</button> -->
                        <button type="button" class="btn btn-warning" v-on:click="createVenta">Agregar</button>
                       <!-- <button type="button" class="btn btn-warning" v-on:click="filtrarProducto">Filtrar</button> -->
                        <button type="button" class="btn btn-warning" v-on:click="cleanCampos">Limpiar</button>
                       <!-- <button type="button" class="btn btn-warning" v-on:click="deleteProducto">Eliminar</button>
                        <button type="button" class="btn btn-warning" v-on:click="comprar">Comprar</button> -->
                        <br /><br />
                        

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
    name: "Venta",
    data: function () {
        return {
            venta_id: 0,
            id_producto: "", //¿está bien así o debería ser solo id?
            nombre_producto: "",
            precio_producto: 0,
            sub_total: 0,
            cantidad_producto: 0,
            fecha_venta: "",
            telefono: 0,
            newVenta: {}, 
            items: [] 
        };
    },


    methods: {
        init: function () {
            if (this.$route.name != "venta") {
                let username = input.venta_id.getItem("current_username");
                this.$router.push({name: "venta", params: { username: 'username' }});
            }
        },


        findVenta: function () {
            this.venta_id = document.getElementById("idventa").value
            let self = this
            axios.get("http://127.0.0.1:8000/venta/consulta/" + this.venta_id)
                .then((result) => {
                    self.venta_id = result.data.venta_id
                    self.id_producto = result.data.id_producto
                    self.nombre = result.data.nombre_producto
                    self.precio = result.data.precio_producto
                    self.cantidad = result.data.cantidad_producto
                    self.subtotal = result.data.sub_total
                    self.fecha = result.data.fecha_venta
                    self.telefono = result.data.telefono
                    
                    document.getElementById("idventa").value = self.venta_id;
                    document.getElementById("idprod").value = self.id_producto;
                    document.getElementById("nomprod").value = self.nombre_producto;
                    document.getElementById("precprod").value = self.precio_producto;
                    document.getElementById("cantprod").value = self.cantidad_producto;
                    document.getElementById("subtotal").value = self.sub_total;
                    document.getElementById("fechaventa").value = self.fecha_venta;   
                    document.getElementById("telefono").value = self.telefono;                
                })
                .catch((error) => {
                    alert("ERROR Servidor");
                });
        },

        createVenta: function () {
            this.venta_id = document.getElementById("idventa").value
            this.id_producto = document.getElementById("idprod").value
            this.nombre_producto = document.getElementById("nomprod").value
            this.precio_producto = document.getElementById("precprod").value
            this.cantidad_producto = document.getElementById("cantprod").value
            this.sub_total = document.getElementById("subtotal").value
            this.fecha_venta = document.getElementById("fechaventa").value
            this.telefono = document.getElementById("telefono").value

            this.newVenta = {
                            "venta_id": this.venta_id,
                            "id_producto": this.id_producto,
                            "nombre_producto": this.nombre_producto,
                            "precio_producto": this.precio_producto,
                            "cantidad_producto": parseInt(this.cantidad_producto),
                            "sub_total": this.sub_total,
                            "fecha_venta": this.fecha_venta,
                            "telefono": parseInt(this.telefono),
            }
            let self = this          
            axios.post("http://127.0.0.1:8000/venta/crear/", this.newVenta)
                .then((result) => {
                    
                    window.confirm("Venta creada");
                    self.items = result.newVenta
                })
                .catch((error) => {
                    alert("ERROR Servidor");
                });
            this.myProvider()
            this.$refs.table.refresh()
        },
        
/* 
        comprar: function () {
            this.venta_id = document.getElementById("idventa").value

            console.log("Entró");
            let self = this
            
            axios.get("http://127.0.0.1:8000/venta/comprar/")
            .then((result) => {
                self.items = result.data
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })
        },*/

        myProvider: function () {
            console.log("Entró");
            let self = this
            
            axios.get("http://127.0.0.1:8000/venta/lista/")
            .then((result) => {
                self.items = result.data
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })
        },

//Aquí me perdí
/*
        filtrarProducto:function () {
            console.log("Entró a buscar");
            this.snombre = document.getElementById("nomprod").value
            this.cat = document.getElementById("catprod").value
            this.id = document.getElementById("idprod").value
            let self = this
            if (this.snombre!=""){

                axios.get("http://127.0.0.1:8000/venta/consulta_n/"+this.snombre)
            .then((result) => {
                self.items = result.data
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })   

            }
            else if (this.cat!="")
                {
            axios.get("http://127.0.0.1:8000/producto/consulta_g/" + this.cat)
                .then((result) => {
                self.items = result.data
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })

                }

            else if(this.id!=""){
                axios.get("http://127.0.0.1:8000/producto/consulta/" + this.id)
                .then((result) => {
                self.items = [result.data]
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })

            }
             
        },
*/
////
/*
        deleteVenta: function () {
            this.idventa = document.getElementById("idventa").value
            this.venta = {
                            "idventa": this.idventa,
                            "idprod": this.idprod,
                            "nombre": this.nombre,
                            "precio": this.precio,
                            "cantidad": this.cantidad,
                            "subtotal": this.subtotal,
                            "fechaventa": this.fechaventa,
                            "telefono": this.telefono,
                            } 
            let idventa = this.venta
            let self = this
            axios.delete("https://restaurante-back-g1.herokuapp.com/venta/delete/", {data: idventa})
                .then((result) => {
                    
                    confirm("La venta se eliminó exitosamente");
                    
                    
                })
                .catch((error) => {
                    alert("ERROR Servidor");
                });
            this.myProvider()
            this.$refs.table.refresh()

        },
*/
        cleanCampos: function () { 
            document.getElementById("idventa").value = ""
            document.getElementById("idprod").value = ""
            document.getElementById("nomprod").value = ""
            document.getElementById("precprod").value = ""
            document.getElementById("cantprod").value = ""
            document.getElementById("subtotal").value = ""
            document.getElementById("fechaventa").value = ""
            document.getElementById("telefono").value = ""
        },
    
    },
}

</script>

<style>
#Venta {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-content: center;
    justify-items: center; 
    margin-bottom: 0%;
}
#Venta h2{
    width: 100%;
    text-align: center;
    margin-top: 7%;
    color:  #fffdfd;
}
#Venta .formulario {
    color:  #fffdfd;
    align-content: center;
    display: block;
    margin-left: 15%;
    font-weight: bold;
    margin-bottom: 1%;
    margin-top: 0%;
}

#Venta button {
    color:  #181818;  
}
#Venta .btn-warning{
    background-color: #f5a018;
}
#Venta .botones {
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
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}
</style>
