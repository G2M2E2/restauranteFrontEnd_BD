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
                    <div class="form-group col-md-4">
                        <b-input-group size="lg" prepend="Venta Id">
                        <b-form-input id="idventadisplay" disabled></b-form-input>
                        </b-input-group>
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
                    <div class="form-group col-md-4">
                        <b-input-group size="lg" prepend="Total $" append=".00" >
                        <b-form-input id="totalpanel" disabled></b-form-input>
                        </b-input-group>
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
                <div class="form-row">
                    <div class="form-group col-md-2">
                        <label for="comentarios">Comentarios:</label>
                    </div>
                    <div class="form-group col-md-2">
                        <input type="text" class="form-control" id="comentarios" name="comentarios" value="" placeholder="Comentarios adicionales"/>
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
                    
                        <button type="button" class="btn btn-warning" @click="myProvider" v-on:click="toggle">Carrito</button> 
                    <!--<button type="button" class="btn btn-warning"  v-on:click="findVenta">Buscar</button> -->
                        <button type="button" class="btn btn-warning" v-on:click="createVenta" >Agregar</button>
                    <!--<button type="button" class="btn btn-warning" v-on:click="filtrarProducto">Filtrar</button>-->
                        <button type="button" class="btn btn-warning" v-on:click="cleanCampos">Limpiar</button>
                        <button type="button" class="btn btn-warning" v-on:click="deleteVenta">Eliminar</button>
                        <button type="button" class="btn btn-warning" v-on:click="comprar">Comprar</button> 
                        <button type="button" class="btn btn-warning" v-on:click="cancelar">Cancelar Venta</button> 

                
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
            @row-clicked="myRowClickHandler"
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
            showTable: false,
            showTable1: false,
            venta_id: 0,
            totalventa: 0,
            id_producto: "", 
            nombre_producto: "",
            precio_producto: 0,
            sub_total: 0,
            cantidad_producto: 0,
            fecha_venta: "",
            id:"",
            telefono: 0,
            comentarios: "",
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
        cancelar: function(){
            document.getElementById("telefono").disabled = false
            document.getElementById("telefono").value = ""
            document.getElementById("totalpanel").value = ""

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
        
        deleteVenta: function () {
            this.id_producto = document.getElementById("idprod").value
            this.telefono = document.getElementById("telefono").value
            this.idventa = document.getElementById("idventadisplay").value
            this.subtotalT = document.getElementById("totalpanel").value
            this.subtotal1 = this.subtotal
            console.log(this.subtotal1)
            console.log(this.subtotalT)
            this.newVenta = {

                            "id_producto": this.id_producto,
                            "telefono": parseInt(this.telefono),
                            "venta_id": this.idventa
            }
            let newVenta = this.newVenta
            let self = this          
            axios.delete("https://restaurante-back-db.herokuapp.com/venta/eliminar/", {data: newVenta})
                .then((result) => {
                    window.confirm("Artículo eliminado");

                })
                .catch((error) => {
                    alert("ERROR Servidor");
                });
            
            this.subtotalT = this.subtotalT - this.subtotal1
            document.getElementById("totalpanel").value = this.subtotalT
            document.getElementById("idprod").value = ""
            this.myProvider()
            this.$refs.table.refresh()
            
        },
        createVenta: function () {
            this.id_producto = document.getElementById("idprod").value
            this.cantidad_producto = document.getElementById("cantprod").value
            this.telefono = document.getElementById("telefono").value
            this.comentarios = document.getElementById("comentarios").value

            this.newVenta = {

                            "id_producto": this.id_producto,
                            "cantidad_producto": parseInt(this.cantidad_producto),
                            "telefono": parseInt(this.telefono),
                            "observaciones": this.comentarios
            }
            let self = this          
            axios.post("https://restaurante-back-db.herokuapp.com/venta/crear/", this.newVenta)
                .then((result) => {
                    window.confirm("Producto agregado");
                    self.items = result.data
                    document.getElementById("idventadisplay").value = result.data[0].venta_id;
                    self.ventas = result.data
                    self.total = 0
                    for (self.subtotal of self.ventas){
                        self.total = self.subtotal.sub_total + self.total
                    self.totalamostrar = self.total.toLocaleString() 
                    document.getElementById("totalpanel").value =  self.total;
                        
                    }
                })
                .catch((error) => {
                    alert("ERROR Servidor");
                });
            
            this.showTable1= false
            this.showTable= true
            this.$refs.table.refresh()
            document.getElementById("telefono").disabled = true
            document.getElementById("idprod").value = ""
            document.getElementById("nomprod").value = ""
            document.getElementById("precprod").value = ""
            document.getElementById("cantprod").value = ""
            document.getElementById("comentarios").value = ""
        },
        

        comprar: function () {

            console.log("Entró");
            let self = this
            
            axios.get("https://restaurante-back-db.herokuapp.com/venta/comprar/")
            .then((result) => {
                self.items = []
                window.confirm("Se realizó la compra satisfactoriamente");
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })
            this.showTable= false
            document.getElementById("telefono").disabled = false
            document.getElementById("telefono").value = ""
            document.getElementById("idventadisplay").value = "";
            document.getElementById("totalpanel").value = "";
        },

        myProvider: function () {
            console.log("Entró");
            let self = this
            
            axios.get("https://restaurante-back-db.herokuapp.com/venta/carrito/")
            .then((result) => {
                self.items = result.data
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })
        },
        cleanCampos: function () { 
            document.getElementById("idprod").value = ""
            document.getElementById("nomprod").value = ""
            document.getElementById("precprod").value = ""
            document.getElementById("cantprod").value = ""
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
        myRowClickHandler(record, index) {
            // 'record' will be the row data from items
            // `index` will be the visible row number (available in the v-model 'shownItems')

            self.id = record.id_producto
            this.subtotal = record.sub_total
            console.log(this.subtotal)
            document.getElementById("idprod").value = self.id;
            
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
