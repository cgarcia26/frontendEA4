<template>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container-fluid">
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
          <router-link to="/login" class="nav-link active">Login</router-link>
          <router-link to="/semesters" class="nav-link active">Semestres</router-link>
          <button class="btn btn-outline-danger logout" type="submit" @click="handleLogout">Logout</button>
        </div>
      </div>
    </div>
  </nav>
    <router-view></router-view>
  </template>
  
  <script>
  export default {
    
        methods:{
            handleLogout(){
                localStorage.clear();
                this.$router.replace({path: '/login'});
            }
        }
    }
  </script>

  <style scoped>
    .logout{
        position: fixed;
        right: 20px;
    }
  </style>