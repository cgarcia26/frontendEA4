<template>
   <h1>
        <i class="fa-solid fa-university" aria-hidden="true"></i>
        Asignaturas
        <router-link :to="'/subjects/create'" v-if="rol">
            <button class="btn btn-outline-success" >
                <i className="fa-solid fa-plus"></i>    
            </button>
        </router-link>
    </h1>

    <table class="table table-striped table-hover align-middle">
        <thead>
            <th>#</th>
            <th>Nombre</th>
            <th>Créditos</th>
            <th>Prerequisitos de Aisgnaturas</th>           
            <th>Acciones</th>
        </thead>
        <tbody>
            <tr v-for="subjects in subjects" :key="subjects.id">
                <td>{{ subjects.id }}</td>
                <td>{{ subjects.name }}</td>
                <td>{{ subjects.credits }}</td>
                <td>{{ subjects.subject_prerequisite }}</td>
                <td>
                    <button class="btn btn-outline-info" @click="subjectInfo(subjects.id)" data-bs-toggle="modal" data-bs-target="#subjectModal">
                        <i className="fa-solid fa-eye"></i>
                    </button>
                    <router-link :to="'/subjects/' + subjects.id + '/edit'" v-if="rol">
                        <button class="btn btn-outline-primary">
                            <i className="fa-solid fa-pen-to-square"></i>
                        </button>
                    </router-link>
                    <button class="btn btn-outline-danger" href="javascript:void(0)" @click="deleteSubject(subjects.id)" v-if="rol">
                        <i className="fa-solid fa-trash"></i>
                    </button>
                </td>
            </tr>
        </tbody>
    </table>

    <div class="modal fade" id="subjectModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                <h2 class="modal-title fs-5" id="exampleModalLabel">Información de la Asignatura</h2>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <form>
                        <div class="mb-3">
                            <label for="name" class="form-label">Nombre</label>
                            <input type="text" class="form-control" id="name" v-model="subject.name" readonly>
                        </div>
                        <div class="mb-3">
                            <label for="credits" class="form-label">Créditos</label>
                            <input type="text" class="form-control" id="credits" v-model="subject.credits" readonly>
                        </div>
                        <div class="mb-3">
                            <label for="subject_prerequisite" class="form-label">Prerequisitos Asignaturas</label>
                            <input type="text" class="form-control" id="subject_prerequisite" v-model="subject.subject_prerequisite" readonly>
                        </div>
                        <div class="mb-3">
                            <label for="autonomous_hours" class="form-label">Horas Autónomas</label>
                            <input type="text" class="form-control" id="autonomous_hours" v-model="subject.autonomous_hours" readonly>
                        </div>
                        <div class="mb-3">
                            <label for="directed_hours" class="form-label">Horas Dirigidas</label>
                            <input type="text" class="form-control" id="directed_hours" v-model="subject.directed_hours" readonly>
                        </div>
                        <div class="mb-3">
                            <label for="semesterName" class="form-label">Semestre</label>
                            <input type="text" class="form-control" id="semesterName" v-model="subject.semesterName" readonly>
                        </div>
                        <div class="mb-3">
                            <label for="userName" class="form-label">Docente</label>
                            <input type="text" class="form-control" id="userName" v-model="subject.userName" readonly>
                        </div>
                    </form>  
                </div>
                <div class="modal-footer">
                <button type="button" class="btn btn-outline-success" data-bs-dismiss="modal">Cerrar</button>
                </div>
            </div>
        </div>
    </div>


</template>
<script>
 export default {
        props:['id'],
        async created(){
            const options = {
                method: "GET",
                headers: { 
                    'Content-Type': 'application/json',
                    'Authorization' : 'Token ' + localStorage.getItem('authToken')
                },
            }
            const response = await fetch("http://localhost:8000/api/subject/" +  this.id + "/semester", options);
            const data = await response.json();
            //console.log(data);
            this.subjects = data.data;
        },
        data(){
            return{
                subjects: [],
                subject: [],
                rol: this.evaluateRol()
            }
        },
       methods:{
            async deleteSubject(id){
                //console.log(id);
                const options = {
                method: "DELETE",
                headers: { 
                    'Content-Type': 'application/json',
                    'Authorization' : 'Token ' + localStorage.getItem('authToken')
                },
            }
                const response = await fetch("http://localhost:8000/api/subject/"+ id +"/destroy", options);
                const data = await response.json();
                // console.log(data.data.newList);
                this.subjectsAll();           
            },
            async subjectsAll(id){
            const options = {
                method: "GET",
                headers: { 
                    'Content-Type': 'application/json',
                    'Authorization' : 'Token ' + localStorage.getItem('authToken')
                },
            }
            const response = await fetch("http://localhost:8000/api/subject/" + this.id + "/semester", options);
            const data = await response.json();
            //console.log(data);
            this.subjects = data.data;
            },
            async subjectInfo(id){
                //console.log(id);
                const options = {
                method: "GET",
                headers: { 
                    'Content-Type': 'application/json',
                    'Authorization' : 'Token ' + localStorage.getItem('authToken')
                },
            }
                const response = await fetch("http://localhost:8000/api/subject/"+ id, options);
                const data = await response.json();
                // console.log(data.data.newList);
                this.subject = data.data;           
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
    h2{
        font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
        text-align: center; 
    }
</style>