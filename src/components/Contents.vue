<template>
  <div class="container">
    <div class="row">
        <div class="col-md-4">
            <div class="input-group mb-3">
                <input type="text" v-model="todosForm.text" ref="todoInput" class="form-control" placeholder="Please type a todo" aria-label="add to-do" aria-describedby="add-todo">
                <div class="input-group-append" v-if="edit">
                    <button @click="updateTodo" class="btn btn-outline-secondary" type="button" id="add-todo">Update</button>
                </div>
                <div class="input-group-append" v-else>
                    <button @click="addTodo" class="btn btn-outline-secondary" type="button" id="add-todo">Add Todo</button>
                </div>
            </div>
        </div>
    </div>
    <div class="row pb-5">
        <div class="col-md">
            <div class="card mb-3">
                <div class="card-header">
                    Things To Do
                </div>
                <div class="card-header">
                    <ul v-for="(todo, key) in todos" :key="key" >
                        <li v-if="todo.status == 'todo'">
                            {{todo.text}}
                            <span class="float-right bg-success" @click="moveToDoing(key)">
                                <img src="@/assets/icons/box-arrow-right.svg" alt="box-arrow-right-logo" width="22" height="22" title="doing">
                            </span>
                            <span class="float-right bg-primary" @click="editTodo(key)">
                                <img src="@/assets/icons/pencil.svg" alt="pencil-logo" width="22" height="22" title="edit">
                            </span>
                            <span class="float-right bg-danger" @click="removeTodo(key)">
                                <img src="@/assets/icons/x.svg" alt="cancel-logo" width="22" height="22" title="remove">
                            </span>
                        </li>
                    </ul>
                </div>
            </div>
        </div>

        <div class="col-md">
            <div class="card mb-3">
                <div class="card-header">
                    Doing
                </div>
                <div class="card-header">
                    <ul v-for="(todo, key) in todos" :key="key" >
                        <li v-if="todo.status == 'doing'">
                            {{todo.text}}
                            <span class="float-right bg-success" @click="moveToDone(key)">
                                <img src="@/assets/icons/check-circle.svg" alt="cancel-logo" width="22" height="22" title="done">
                            </span>
                            <span class="float-right bg-danger" @click="removeTodo(key)">
                                <img src="@/assets/icons/x.svg" alt="cancel-logo" width="22" height="22" title="remove">
                            </span>
                            <span class="float-right bg-primary" @click="moveToDo(key)">
                                <img src="@/assets/icons/box-arrow-left.svg" alt="cancel-logo" width="22" height="22" title="todo">
                            </span>
                        </li>
                    </ul>
                </div>
            </div>
        </div>

        <div class="col-md">
            <div class="card mb-3">
                <div class="card-header">
                    Done
                </div>
                <div class="card-header">
                    <ul v-for="(todo, key) in todos" :key="key" >
                        <li v-if="todo.status == 'done'">
                            {{todo.text}}
                            <span class="float-right bg-danger" @click="removeTodo(key)">
                                <img src="@/assets/icons/x.svg" alt="cancel-logo" width="22" height="22" title="remove">
                            </span>
                            <span class="float-right bg-primary" @click="moveToDoing(key)">
                                <img src="@/assets/icons/box-arrow-left.svg" alt="cancel-logo" width="22" height="22" title="doing">
                            </span>
                        </li>
                    </ul>
                </div>
            </div>
        </div>

    </div>
  </div>
</template>

<script>
    import Swal from 'sweetalert2'
    export default {
        data(){
            return {
                todos: [
                    {
                        text: 'Read javascript promises',
                        status: 'todo'
                    },
                    {
                        text: 'How to consume API',
                        status: 'doing'
                    },
                    {
                        text: 'Deploying Heroku',
                        status: 'todo'
                    },
                    {
                        text: 'Prepare for AWS Exams',
                        status: 'done'
                    }
                ],
                todosForm: {
                    text: null,
                    status: 'todo'
                },
                edit: false,
                index: null
            }
        },
        methods: {
            addTodo(){
                if(this.$refs.todoInput.value){
                    const Toast = Swal.mixin({
                        toast: true,
                        position: 'top',
                        showConfirmButton: false,
                        timer: 3000,
                        timerProgressBar: true,
                        onOpen: (toast) => {
                            toast.addEventListener('mouseenter', Swal.stopTimer)
                            toast.addEventListener('mouseleave', Swal.resumeTimer)
                        }
                    })
                    Toast.fire({
                        icon: 'success',
                        title: 'ToDo Added Successfully'
                    })
                    this.todos.unshift(this.todosForm);
                    this.todosForm = {
                        text: null,
                        status: 'todo'
                    }
                }else{
                    const Toast = Swal.mixin({
                        toast: true,
                        position: 'top',
                        showConfirmButton: false,
                        timer: 3000,
                        timerProgressBar: true,
                        onOpen: (toast) => {
                            toast.addEventListener('mouseenter', Swal.stopTimer)
                            toast.addEventListener('mouseleave', Swal.resumeTimer)
                        }
                    })  
                    Toast.fire({
                        icon: 'error',
                        title: 'Fields can not be empty!'
                    })
                }
            },
            removeTodo(index){
                Swal.fire({
                    title: 'Are you sure?',
                    text: "You want to remove this item!",
                    icon: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, remove!'
                }).then((result) => {
                    if (result.value) {
                        Swal.fire(
                            'Deleted!',
                            'todo has been deleted.',
                            'success'
                        )
                        this.todos.splice(index,1);
                    }
                })
            },
            moveToDo(index){
                this.todos[index].status = 'todo';
            },
            moveToDoing(index){
                this.todos[index].status = 'doing';
            },
            moveToDone(index){
                this.todos[index].status = 'done';
            },
            editTodo(index){
                this.edit = true;
                this.index = index;
                this.todosForm = this.todos[index];
            },
            updateTodo(){
                this.todosForm = this.todos[this.index];
                this.edit = false;
                this.todosForm = {
                    text: null,
                    status: 'todo'
                }
            }
        }
    }
</script>

<style lang="scss" scoped>
    ul {
        list-style-type: none;
        padding-left: 0px;
        
        li{
            padding: 5px;
            background-color: wheat;
            border-radius: 5px 10px;
            font-size: 15px;
        }
    }
    span{
        border-radius: 5px;
        img {
            cursor: pointer;
            filter: invert(1);
        }
        // margin-right: 5px;
    }
</style>