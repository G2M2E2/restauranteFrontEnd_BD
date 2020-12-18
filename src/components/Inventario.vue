<template>
    <div id="Inventario">
        <br />
        <h2 >MÓDULO DE INVENTARIO</h2>
        <br />
        
        <form>
            <div class = "formulario" >

                <div class="form-row" >
                    <div class="form-group col-md-5">
                        <label for="idprod">Id:</label>
                        <input type="text" class="form-control" id="idprod" name="idprod" value=""  placeholder="Id"/>
                    </div>
                    <div class="form-group col-md-5">
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
                    <div class="form-group col-md-4">
                        <label for="catprod">Categoría:</label>
                        <!--<input type="text" class="form-control" id="catprod" name="catprod" value="" placeholder="Categoría"/>-->
                        <!--Para opciones desplegables: -->
                        <b-form-select id="catprod" v-model="selected" :options="options" ></b-form-select>
                    </div>
                </div>
            </div>
            <br>
            <div class="botones">
                <div style='text-align:center'>
                    <right>
                        <button type="button" class="btn btn-warning" v-on:click="myProvider">Lista</button>
                        <button type="button" class="btn btn-warning"  v-on:click="findProducto">Buscar</button>
                        <button type="button" class="btn btn-warning" v-on:click="createProducto">Crear</button>
                        <!-- <button type="button" class="btn btn-warning" >Actualizar</button> -->
                        <button type="button" class="btn btn-warning" v-on:click="cleanCampos">Limpiar</button>
                        <button type="button" class="btn btn-warning" v-on:click="deleteProducto">Eliminar</button><br /><br />
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
    name: "Inventario",
    data: function () {
        return {
            id: "",
            nombre: "",
            precio: 0,
            cantidad: 0,
            categoria: "",
            newProducto: {},
            items: [],
            /////para agregar las opciones desplegables/////
            selected: null,
            options: [
                {value: null, text: 'Seleccione una opción'},
                {value: 'acompanante', text: 'Acompañante'},
                {value: 'arroz_pasta', text: 'Arroz o pasta'},
                {value: 'bebidas', text: 'Bebidas'},
                {value: 'carnes', text: 'Carnes'},
                {value: 'cerdo', text: 'Cerdo'},
                {value: 'crepes', text: 'Crepes'},
                {value: 'desayunos', text: 'Desayunos'},
                {value: 'ensalada', text: 'Ensalada'},
                {value: 'entradas', text: 'Entradas'},
                {value: 'hamburguesa', text: 'Hamburguesa'},
                {value: 'menu infantil', text: 'Menú infantil'},
                {value: 'patacon', text: 'Patacón'},
                {value: 'pescados', text: 'Pescados'},
                {value: 'picada', text: 'Picada'},
                {value: 'pinchos', text: 'Pinchos'},
                {value: 'pollo', text: 'Pollo'},
                {value: 'tipicos', text: 'Típicos'},
                {value: 'varios', text: 'Varios'},
                {value: 'vinos', text: 'Vinos'},
            ]

        };
    },
    methods: {
        init: function () {
        if (this.$route.name != "inventario") {
            let username = input.idprod.getItem("current_username");
            this.$router.push({name: "inventario", params: { username: 'username' }});
        }
        },
        findProducto: function () {
            this.id = document.getElementById("idprod").value
            let self = this
            axios.get("https://restaurante-back-g1.herokuapp.com/producto/consulta/" + this.id)
                .then((result) => {
                    self.id = result.data.id
                    self.nombre = result.data.nombre
                    self.precio = result.data.precio
                    self.cantidad = result.data.cantidad
                    self.categoria = result.data.categoria
                    
                    document.getElementById("idprod").value = self.id;
                    document.getElementById("nomprod").value = self.nombre;
                    document.getElementById("precprod").value = self.precio;
                    document.getElementById("cantprod").value = self.cantidad;
                   //document.getElementById("catprod").value = self.categoria;   
                   self.selected = self.categoria;              
                })
                .catch((error) => {
                    alert("ERROR Servidor");
                });
        },
        createProducto: function () {
            this.id = document.getElementById("idprod").value
            this.nombre = document.getElementById("nomprod").value
            this.precio = document.getElementById("precprod").value
            this.cantidad = document.getElementById("cantprod").value
            this.categoria = document.getElementById("catprod").value
            this.newProducto = {
                            "id": this.id,
                            "nombre": this.nombre,
                            "precio": parseInt(this.precio),
                            "cantidad": parseInt(this.cantidad),
                            "categoria": this.categoria,
            }   
            let self = this          
            axios.post("https://restaurante-back-g1.herokuapp.com/producto/crear/", this.newProducto)
                .then((result) => {
                    window.confirm("Producto Creado");
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
            
            axios.get("https://restaurante-back-g1.herokuapp.com/producto/lista/")
            .then((result) => {
                self.items = result.data
            }).catch(error => {
                
                alert("ERROR Servidor");
                return []
            })
        },
        deleteProducto: function () {
            this.id = document.getElementById("idprod").value
            this.producto = {
                            "id": this.id,
                            "nombre": this.nombre,
                            "precio": this.precio,
                            "cantidad": this.cantidad,
                            "categoria": this.categoria
                            } 
            let id = this.producto
            let self = this
            axios.delete("https://restaurante-back-g1.herokuapp.com/producto/delete/", {data: id})
                .then((result) => {
                    
                    confirm("El producto se eliminó exitosamente");
                        
                })
                .catch((error) => {
                    alert("ERROR Servidor");
                });
            this.myProvider()
            this.$refs.table.refresh()
        },
        cleanCampos: function () {
            
            document.getElementById("idprod").value = ""
            document.getElementById("nomprod").value = ""
            document.getElementById("precprod").value = ""
            document.getElementById("cantprod").value = ""
            document.getElementById("catprod").value = ""
                                
        },
    },
}
</script>

<style>
#Inventario {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-content: center;
    justify-items: center; 
    margin-bottom: 0%;
}
#Inventario h2{
    width: 100%;
    text-align: center;
    margin-top: 1%;
    color:  #fffdfd;
}
#Inventario .formulario {
    color:  #fffdfd;
    align-content: center;
    display: block;
    margin-left: 15%;
    font-weight: bold;
    margin-bottom: 1%;
}
#Inventario button {
    color:  #181818;  
}
#Inventario .btn-warning{
    background-color: #f5a018;
}
#Inventario .botones {
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