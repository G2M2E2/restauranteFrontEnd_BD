<template>

    <div id="AuthUser" class="auth_user">
        <br />
        <br />
        <br />
        <div class="container_auth_user">
            <h2>Autenticarse</h2>

            <form v-on:submit.prevent="processAuthUser" >
                
                <input type="text" v-model="user_in.username" placeholder="Username">
                <br>
                <input type="password" v-model="user_in.password" placeholder="Password">
                <br>
                <button type="submit">Iniciar Sesión</button>

            </form>
        </div>

    </div>


</template>


<script>
import axios from 'axios';

export default {
    name: "UserAuth",

    data: function(){
        return {
            user_in: {
                username:"",
                password:""
            }
        }
    },

    methods: {
        processAuthUser: function(){
            var self = this
            axios.post("https://restaurante-back-db.herokuapp.com/usuario/auth/", self.user_in,  {headers: {}})
                .then((result) => {
                    alert("Autenticación Exitosa");
                    self.$emit('log-in', self.user_in.username)
                })
                .catch((error) => {
                    
                    if (error.response.status == "404")
                        alert("ERROR 404: Usuario no encontrado.");
                    
                    if (error.response.status == "403")
                        alert("ERROR 403: Contraseña Erronea.");  
                });
        }
    }
}
</script>

<style>
    .auth_user{
        margin: 0;
        padding: 0%;
        height: 100%;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .auth_user h2{
        font-size: 1.6rem;
    }
    .container_auth_user {
        border: 3px solid  #f5a018;
        border-radius: 10px;
        width: 25%;
        height: 60%;
        margin-top: 10%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding: 2%;
    }
    .auth_user h2{
        color: #f5a018;
    }
    .auth_user form{
        width: 50%;   
    }
    .auth_user input{
        height: 40px;
        width: 100%;
        box-sizing: border-box;
        padding: 10px 20px;
        margin: 5px 0;
        font-size: 90%;
        text-align: center;
        border: 1px solid #283747;
    }
    .auth_user button{
        width: 100%;
        height: 40px;
        color: #E5E7E9;
        background: #f5a018;
        border: 1px solid #E5E7E9;
        border-radius: 5px;
        padding: 1% 3%;
        margin-top: 6%;
        font-size: 90%;
        line-height: 95%;
    }
    .auth_user button:hover{
        color: #E5E7E9;
        background: crimson;
        border: 1px solid #283747;
    }
</style>
