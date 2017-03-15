<template>
    <div id="boards">
        <div class="row">
            <h1 class="center">Your Boards</h1>
            <div class="col xs12 s6 m2" v-for="(board, i) in userBoards">
                <div class="card-panel teal">
                    <span class="white-text" @click="getBoard(board)" v-if="!edit">{{board.name}}
                    </span>
                    <input :value="name" placeholder="name" v-model="name" v-if="edit">
                    <i id="right" class="tiny material-icons" v-on:click="show = !show">mode_edit</i><br v-if="show">
                    <br>
                    <a class="black-text" id="right" @click="deleteBoard(board)" v-if="show">Del</a>
                    <a class="black-text" id="left" v-if="show" v-on:click="edit = !edit">Edit</a>
                </div>
            </div>
           <boardForm></boardForm>
        </div>
        <collab></collab>
    </div>
</template>




<script>
    import boardForm from './BoardForm'
    import collab from './CollabBoards'
    import router from '../router'
    export default {
        name: 'boards',
        components: { boardForm, collab },
        data(){
            return{
            show: false,
            edit: false,
            name: this.board
            }
        },
        mounted() {
            this.$root.$data.store.actions.getUserBoards()
            this.$root.$data.store.actions.getCollabBoards()
        },
        computed: {
            userBoards() {
                return this.$root.$data.store.state.userBoards
            },

        },
        methods: {
            getBoard(board){
                this.$root.$data.store.actions.setBoard(board)
                
                router.push('/board')
            },
            createBoard() {
                this.$root.$data.store.actions.getUserBoards()
            },
            deleteBoard(board) {
                let x = board._id
                this.$root.$data.store.actions.deleteBoard(x)
                
            }
        }
    }

</script>




<style>
    .center {
        text-align: center
    }
    
    #right {
        float: right;
    }
</style>