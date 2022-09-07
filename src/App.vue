<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-sm-6">
                <Add :todo='todo' :isEdit='isEdit'/>
            </div>
            <div class="col-sm-6">
                <ListAll :todos='todos'/>
            </div>
        </div>
    </div>    
</template>
<script>
    import axios from 'axios';
    import Add from './components/todo/Add.vue';
    import ListAll from './components/todo/ListAll.vue';
    export default {
        name: 'App',
        data(){
            return{
                todos:[],
                todo:{
                    id:'',
                    title:'',
                    description:''  
                },
                isEdit:false
            }
        },
        mounted(){
            this.fetchAllTodos();
        },
        methods:{
            async fetchAllTodos(){
                await axios.get('http://localhost:3000/todos')
                .then( (response)=> {
                    this.todos=response.data
                })
                .catch(function (error) {
                    console.log(error);
                });  
            },
            async editTodo(id){

                await axios.get('http://localhost:3000/todos/'+id)
                .then( (response)=> {
                    this.todo.title=response.data.title
                    this.todo.description=response.data.description
                    this.todo.id=response.data.id
                    this.isEdit=true
                })
                .catch(function (error) {
                    console.log(error);
                });  
            }
        },
        components:{
            Add,
            ListAll
        }
    }
</script>