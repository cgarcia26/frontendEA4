<template>
    <h1>
        <i class="fa fa-sign-in" aria-hidden="true"></i>
        Login
    </h1>
    <form @submit.prevent="submitUser">
        <div class="mb-3">
            <label for="email" class="form-label">Email</label>
            <input type="email" v-model="user.email" class="form-control" id="email" aria-describedby="email" required>
        </div>
        <div class="mb-3">
            <label for="password" class="form-label">Password</label>
            <input type="password" v-model="user.password" class="form-control" id="password" required>
        </div>
        <button type="submit" class="btn btn-primary">Ingresar</button>
    </form>
</template>
<script>
    export default {
        data(){
            return {
                user: {
                    email: '',
                    password: ''
                }
            }
        },
        methods:{
            async submitUser(){
                const options = {
                    method: "POST",
                    headers: { 
                        'Content-Type': 'application/json'
                    },
                    body:  JSON.stringify(this.user)
                }
                const response = await fetch("http://127.0.0.1:8000/api/user/login", options);
                const data = await response.json();
                if(data.code == 200 ){
                    localStorage.setItem('authToken', data.token)
                    localStorage.setItem('rol', data.rol)
                    this.$router.replace({path: '/semesters'});
                }else{
                    window.alert("Usuario o contraseña incorrectos");
                }
            }
        }
    }
</script>
<style>
    form{
        width: 400px;
        margin: 2em auto;
    }
    h1{
        font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
        text-align: center;
        text-shadow: 1px 1px 1px black, 2px 2px 1px green;  
    }
</style>