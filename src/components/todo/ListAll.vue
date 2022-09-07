<template>
    <h3>All Todos</h3>
    <ul>
        <li v-for="todo in todos" :key="todo.id">{{todo.title}} <a href="#" v-on:click="deleteTodo(todo.id)">Delete</a> | <a href="#" v-on:click="editTodo(todo.id)">Edit</a></li>
    </ul>
</template>
<script>
    
import axios from 'axios';
export default {
    props: ['todos'],
    data(){
        return{
        
        }
    },
    mounted(){
        
    },
    methods:{
        async deleteTodo(id)
        {
            await axios.delete('http://localhost:3000/todos/'+id)
                .then( (response)=> {
                    console.log(response);
                    this.$parent.fetchAllTodos();
                })
                .catch(function (error) {
                    console.log(error);
                }); 
        },
        editTodo(id){
            this.$parent.editTodo(id);
        }

    }
}
</script>