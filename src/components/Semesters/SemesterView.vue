<template>
    <h1>
        <i class="fa-solid fa-graduation-cap" aria-hidden="true"></i>
        Semestres
        <router-link :to="'create'" v-if="rol">
            <button class="btn btn-outline-success">
                <i className="fa-solid fa-plus"></i>    
            </button>
        </router-link>
    </h1>

    <table class="table table-striped table-hover align-middle">
        <thead>
            <th>#</th>
            <th>Nombre</th>
            <th>Acciones</th>
        </thead>
        <tbody>
            <tr v-for="semesters in semesters" :key="semesters.id">
                <td>{{ semesters.id }}</td>
                <td>{{ semesters.name }}</td>
                <td>
                    <router-link :to="'/subjects/' + semesters.id + '/semester'">
                        <button class="btn btn-outline-info">
                            <i className="fa-solid fa-eye"></i>
                        </button>
                    </router-link>
                    <router-link :to=" semesters.id + '/edit'" v-if="rol">
                        <button class="btn btn-outline-primary">
                            <i className="fa-solid fa-pen-to-square"></i>
                        </button>
                    </router-link>
                    <button class="btn btn-outline-danger" href="javascript:void(0)" @click="deleteSemester(semesters.id)" v-if="rol">
                        <i className="fa-solid fa-trash"></i>
                    </button>
                </td>
            </tr>
        </tbody>
    </table>
</template>

<script>
    export default {
       async created(){
            const options = {
                method: "GET",
                headers: { 
                    'Content-Type': 'application/json',
                    'Authorization' : 'Token ' + localStorage.getItem('authToken')
                },
            }
            const response = await fetch("http://localhost:8000/api/semester", options);
            const data = await response.json();
            //console.log(data);
            this.semesters = data.data;
        },
        data(){
            return{
                semesters: [],
                rol: this.evaluateRol()
            }
        },
       methods:{
            async deleteSemester(id){
                //console.log(id);
                const options = {
                method: "DELETE",
                headers: { 
                    'Content-Type': 'application/json',
                    'Authorization' : 'Token ' + localStorage.getItem('authToken')
                },
            }
                const response = await fetch("http://localhost:8000/api/semester/"+ id +"/destroy", options);
                const data = await response.json();
                // console.log(data.data.newList);
                this.semestersAll();           
            },
            async semestersAll(){
            const options = {
                method: "GET",
                headers: { 
                    'Content-Type': 'application/json',
                    'Authorization' : 'Token ' + localStorage.getItem('authToken')
                },
            }
            const response = await fetch("http://localhost:8000/api/semester", options);
            const data = await response.json();
            //console.log(data);
            this.semesters = data.data;
            },
            evaluateRol(){

                this.rol = localStorage.getItem('rol')

                if( this.rol == 3){
                    return true
                }else{
                    return false
                }
            }
       }
    }
</script>

<style scoped>
    button{
        margin: 1%
    }
    h1{
        font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
        text-align: center;
        text-shadow: 1px 1px 1px black, 2px 2px 1px green;  
    }
</style>