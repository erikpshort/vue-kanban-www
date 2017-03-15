<template>
    <div class="col xs12 s6 m2" droppable="true" v-on:drop.capture="addTask" ondragover="event.preventDefault()">
        <div class="card-panel teal white-text">
            <input :value="list.name" :placeholder="list.name" v-model="name" v-on:keyup.enter="changeListName(name)">
            <task v-for="(task, i) in tasks" :task="task" draggable="true" v-on:dragstart.capture="moving"></task>
            <taskForm :list="list"></taskForm>
            <a class="right black-text" @click="deleteList(list)">Delete List</a>
        </div>
    </div>
</template>




<script>
    import task from './Task'
    import taskForm from './TaskForm'

    export default {
        name: 'lists',
        props: ['list'],
        components: { task, taskForm },
        data() {
            return {
                name: this.list.name
            }
        },
        computed: {
            tasks() {
                let vm = this
                return this.$root.$data.store.state.tasks.filter(function (task) {
                    return task.listId == vm.list._id
                })
            },
        },
        methods: {
            addTask(event) {
                var task = JSON.parse(event.dataTransfer.getData('text/javascript'))
                var id = task._id
                var object = { listId: this.list._id }
                var boardId = this.list.boardId
                this.$root.$data.store.actions.changeTask(boardId, object, id)
                console.log("We've Just Landed")
            },
            deleteList(list) {
                var x = list._id
                var y = list.boardId
                this.$root.$data.store.actions.deleteList(x, y)
            },
            changeListName(name){
                var obj = { name: this.name}
                var boardId = this.list.boardId
                var listId = this.list._id
                this.$root.$data.store.actions.changeList(listId, obj, boardId)
            }
        },
        mounted() {
            this.$root.$data.store.actions.getTasks(this.$root.$data.store.state.activeBoard._id)

        },
    }

</script>




<style>
    .right {
        float: right;
        bottom: 0
    }
</style>