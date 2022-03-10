<template>
    <div>
        <ul>
            <li v-for="todo in todos" :key="todo.id">
                <!-- {{todo.created}} -->
                <!-- {{todo.name}} -->
                <span v-if="todo.created">
                    <input 
                    type="checkbox"
                    v-bind:checked="todo.done"
                    @change="toggle(todo)">
                    <span v-bind:class="{ done:todo.done }">
                        {{todo.created}}
                        {{todo.name}} {{formatDate(todo.created)}}
                    </span>
                    <button v-on:click="remove(todo.id)">X</button>
                </span>
                test
            </li>
        </ul>
        <div class="form">
            <form v-on:submit.prevent="add">
                <input v-model="name">
                <button>Add</button>
            </form>
        </div>
    </div>
</template>
<script>

import moment from 'moment'
export default {
    data: function() {
        return {
            name: '',
            done: false
        }
    },
    created: function() {
        this.$store.dispatch('todos/init')
    },
    methods: {
        add(){
            this.$store.dispatch('todos/add', this.name)
            this.name = ''
        },
        remove(id) {
            this.$store.dispatch('todos/remove', id)
        },
        toggle(todo) {
            this.$store.dispatch('todos/toggle', todo)
        },
        // formatDate(date) {
        //     // console.log(date);
        //     d1 = Number(date);
        //     console.log(d1);
        //     return moment(d1).format('YYYY/MM/DD HH:mm:ss')
        // }
    },
    computed: {
        todos() {
            // return this.$store.state.todos.todos
            return this.$store.getters['todos/orderedTodos']
        },
        
        formatDate: function(){
            return function(dt) {
                console.log(dt.toDate());
                return moment(dt.toDate()).format('YYYY/MM/DD HH:mm:ss')
            }
        }
    }
}
</script>
<style>
    li > span > span.done {
        text-decoration: line-through;
    }
</style>

