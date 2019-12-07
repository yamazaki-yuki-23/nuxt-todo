<template>
    <div>
        <ul>
            <li v-for="todo in todos" :key="todo.id">
                <span v-if="todo.created">
                    <input type="checkbox" v-bind:checked="todo.done" @change="toggle(todo)">
                    <span v-bind:class="{ done: todo.done }">
                        {{ todo.name }} {{todo.created.toDate() | dateFilter }}
                    </span>
                    <button class="button--grey" v-on:click="remove(todo.id)">削除</button>
                </span>
            </li>
        </ul>
        <div class="form">
            <form v-on:submit.prevent="add">
                <input v-model="name">
                <button class="button--green">追加</button>
            </form>
        </div>
    </div>
</template>

<script>
import moment from 'moment'
export default {
    data() {
        return {
            name: '',
            done: false
        }
    },
    created:function(){
        this.$store.dispatch('todos/init')
    },
    methods: {
        add(){
            this.$store.dispatch('todos/add', this.name)
            this.name = ''
        },
        remove(id){
            this.$store.dispatch('todos/remove', id)
        },
        toggle(todo){
            this.$store.dispatch('todos/toggle', todo)
        }
    },
    computed: {
        todos() {
            return this.$store.getters['todos/orderdTodos']
        }
    },
    filters:{
        dateFilter: function(date){
            return moment(date).format('YYYY/MM/DD')
        }
    }
}
</script>

<style>
    li > span > span.done{
        text-decoration: line-through;
    }
    ul {
        list-style: none;
    }
    .form{
        margin-top: 10px;
        margin-left: 40px;
    }
</style>