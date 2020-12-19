<template>
    <div id="Venta">

        <h2 >MÓDULO DE VENTAS</h2>
        <br />
        
        <form>
            <div class = "formulario" >

                <div class="form-row" >
                    <div class="form-group col-md-2">
                        <label for="telefono">Teléfono cliente:</label>
                    </div>
                    <div class="form-group col-md-2">
                        <input type="number" class="form-control" id="telefono" name="telefono" value="" placeholder="Teléfono"/>
                    </div>
                        <div class="form-group col-md-2">
                        <button type="button" class="btn btn-warning"  v-on:click="findCliente">Buscar</button>
                    </div>
                    
                    </div>
                <div class="form-row">
                    <div class="form-group col-md-2">
                        <label for="nomprod">Nombre producto:</label>
                    </div>
                    <div class="form-group col-md-2">
                        <input type="text" class="form-control" id="nomprod" name="nomprod" value="" placeholder="Nombre"/>
                    </div>
                    
                    
                    <div class="form-group col-md-2">
                        <button type="button" class="btn btn-warning" v-on:click="filtrarProducto">Filtrar</button>
                    </div>

                </div>
                <div class="form-row">
                    <div class="form-group col-md-2">
                        <label for="idprod">Id Producto:</label>
                    </div>
                    <div class="form-group col-md-2">
                        <input type="text" class="form-control" id="idprod" name="idprod" value=""  placeholder="Id_producto"/>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group col-md-2">
                        <label for="precprod">Precio:</label>
                    </div>
                    <div class="form-group col-md-2">
                        <input type="number" class="form-control" id="precprod" name="precprod" value="" placeholder="Precio"/>  
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group col-md-2">
                        <label for="cantprod">Cantidad:</label>
                    </div>
                    <div class="form-group col-md-2">
                        <input type="number" class="form-control" id="cantprod" name="cantprod" value="" placeholder="Cantidad"/>
                    </div>
                </div>
            </div>
            <b-table 
                v-show="showTable1" 
                :fields="fields1" 
                sticky-header 
                ref="table1" 
                id="my-table1" 
                striped hover 
                @row-clicked="myRowClickHandler1"
                :items="items1"
            >


        </b-table>
            <br>
            <div class="botones">
                <div style='text-align:center'>
                    
                        <button type="button" class="btn btn-warning" @click="myProvider" v-on:click="toggle">Lista</button> 
                    <!--<button type="button" class="btn btn-warning"  v-on:click="findVenta">Buscar</button> -->
                        <button type="button" class="btn btn-warning" v-on:click="createVenta" >Agregar</button>
                        <!-- <button type="button" class="btn btn-warning" v-on:click="filtrarProducto">Filtrar</button> -->
                        <button type="button" class="btn btn-warning" v-on:click="cleanCampos">Limpiar</button>
                        <!-- <button type="button" class="btn btn-warning" v-on:click="deleteProducto">Eliminar</button>-->
                        <button type="button" class="btn btn-warning" v-on:click="comprar">Comprar</button> 
                        

                
                </div>
            </div>
        
        </form>
        <br />
        
        <b-table 
            v-show="showTable" 
            :fields="fields" 
            sticky-header 
            ref="table" 
            id="my-table" 
            striped hover 
            :items="items">
        </b-table>

        <span class="label label-default">Total = {{totalventa}}</span>
        
    </div>
</template>

<script>

import axios from "axios";
export default {
    name: "Venta",
    data: function () {
        return {
            showTable: true,
            showTable1: true,
            venta_id: 0,
            totalventa: 0,
            id_producto: "", //¿está bien así o debería ser solo id?
            nombre_producto: "",
            precio_producto: 0,
            sub_total: 0,
            cantidad_producto: 0,
            fecha_venta: "",
            id:"",
            telefono: 0,
            precio: 0,
            nombre: "",
            newVenta: {}, 
            items: [],
            items1: [],
            fields: [
                { key: 'id_producto', label: 'Id Producto', sortable: true, sortDirection: 'desc' },
                { key: 'nombre_producto', label: 'Nombre', sortable: true, class: 'text-center' },
                { key: 'precio_producto', label: 'Precio', sortable: true, class: 'text-center' },
                { key: 'cantidad_producto', label: 'Cantidad', sortable: true, class: 'text-center' },
                { key: 'sub_total', label: 'Sub Total', sortable: true, class: 'text-center' },
            ],
            fields1: [
                { key: 'id_producto', label: 'Id Producto', sortable: true, sortDirection: 'desc' },
                { key: 'nombre', label: 'Nombre', sortable: true, class: 'text-center' },
                { key: 'precio', label: 'Precio', sortable: true, class: 'text-center' },
            ], 
        };
    },


    methods: {
        init: function () {
            if (this.$route.name != "venta") {
                let username = input.venta_id.getItem("current_username");
                this.$router.push({name: "venta", params: { username: 'username' }});
            }
        },
        toggle: function() {
        this.showTable = !this.showTable;
        },
        findVenta: function () {
            this.venta_id = document.getElementById("idventa").value
            let self = this
            axios.get("https://restaurante-back-db.herokuapp.com/venta/consulta/" + this.venta_id)
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
            this.id_producto = document.getElementById("idprod").value
            this.cantidad_producto = document.getElementById("cantprod").value
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
            axios.post("https://restaurante-back-db.herokuapp.com/venta/crear/", this.newVenta)
                .then((result) => {
                    console.log(result.data)
                    window.confirm("Venta creada");
                    self.items = result.data
                    self.subtotal1 = result.data.sub_total
                })
                .catch((error) => {
                    alert("ERROR Servidor");
                });
            this.showTable1= false
            this.totalventa= this.totalventa+this.subtotal1
            this.showTable= true
            this.$refs.table.refresh()
        },
        

        comprar: function () {

            console.log("Entró");
            let self = this
            
            axios.get("https://restaurante-back-db.herokuapp.com/venta/comprar/")
            .then((result) => {
                self.items = []
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })
            this.showTable= false
        },

        myProvider: function () {
            console.log("Entró");
            let self = this
            
            axios.get("https://restaurante-back-db.herokuapp.com/venta/lista/")
            .then((result) => {
                self.items = result.data
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })
        },
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
        filtrarProducto:function () {
            console.log("Entro a buscar");
            this.snombre = document.getElementById("nomprod").value
            this.id = document.getElementById("idprod").value
            let self = this
            if (this.snombre!=""){
                axios.get("https://restaurante-back-db.herokuapp.com/producto/consulta_n/"+ this.snombre)
            .then((result) => {
                console.log(result.data)
                self.items1 = result.data

            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })   
            }
            else if(this.id!=""){
                axios.get("https://restaurante-back-db.herokuapp.com/producto/consulta/" + this.id)
                .then((result) => {
                self.items1 = [result.data]
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })
            }
            this.showTable1= true
        },
        findCliente: function () {
            this.telefono = document.getElementById("telefono").value
            let self = this
            axios.get("https://restaurante-back-db.herokuapp.com/cliente/consulta/" + this.telefono)
                .then((result) => {
                    self.telefono = result.data.telefono
                    self.nombre = result.data.nombre
                    confirm("Se encontró el cliente " + self.nombre);
                    
                })
                .catch((error) => {
                    alert("No se encontró el cliente");
                });

        },
        myRowClickHandler1(record, index) {
            // 'record' will be the row data from items
            // `index` will be the visible row number (available in the v-model 'shownItems')

            self.id = record.id_producto
            self.nombre = record.nombre
            self.precio = record.precio
            
            document.getElementById("idprod").value = self.id;
            document.getElementById("nomprod").value = self.nombre;
            document.getElementById("precprod").value = self.precio;  
            
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
    margin-top: 2%;
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
