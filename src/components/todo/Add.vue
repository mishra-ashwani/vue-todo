<template>
    <h3>Add New Todo</h3>
    <div class="p-5 my-4 bg-light rounded-3">
        <form>
            <div class="form-group">
                <label>Title</label>
                <input type="text" class="form-control" placeholder="Enter title" v-model="todo.title">
            </div>
            <div class="form-group">
                <label>Description</label>
                <textarea placeholder="Description"  class="form-control" name="description" v-model="todo.description"></textarea>
            </div>
            
            <button type="button" class="btn btn-primary" v-if="!isEdit" v-on:click="createTotdo">Submit</button>
            <button type="button" class="btn btn-primary" v-if="isEdit" v-on:click="updateTotdo(todo.id)">Update</button>
        </form>
    </div>
    
</template>
<script>
import axios from 'axios';
export default {
    props: ['todo','isEdit'],
    methods:{ 
        validateFields(){
            if(this.todo.title == ''){
                alert('Please enter title.');
                return false;
            }
            if(this.todo.description == ''){
                alert('Please enter description.');
                return false;
            }
            return true;
        },
        resetForm(){
            this.todo.title='';
            this.todo.description=''
        },

        async createTotdo(){
            if(this.validateFields()){
                await axios.post('http://localhost:3000/todos',{'title':this.todo.title,'description':this.todo.description})
                .then( (response)=> {
                    this.$parent.fetchAllTodos();
                    this.resetForm()
                })
                .catch(function (error) {
                    console.log(error);
                });  
            }     
        },
        async updateTotdo(id){
            if(this.validateFields()){
                await axios.put('http://localhost:3000/todos/'+id,{'title':this.todo.title,'description':this.todo.description})
                .then( (response)=> {
                    this.$parent.fetchAllTodos();
                    this.resetForm()
                })
                .catch(function (error) {
                    console.log(error);
                });  
            }

            this.resetForm();
            this.$parent.isEdit=false;
        }
    }
}
</script>