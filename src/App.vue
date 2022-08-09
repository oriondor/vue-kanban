<template>
    <img alt="Vue logo" src="./assets/logo.png">
    <v-kaban v-if="boards" ref="kanban"
             host="http://localhost:8081"
             path="/kanban"
             v-model="boards"
             :use-custom-card-style="false"
             @taskMoved="taskMoveHandler"
             ordering-type="linkedList"
             @changedElements="changedHandler"
             @taskClicked="clickedOnTask"
             @addNewTask="addNewTask"
             @listSettings="listSettings"
    >
    </v-kaban>
</template>

<script>

import VKaban from "@/components/vKaban";

export default {
    name: 'App',
    components: {
        VKaban
    },
    data(){
        return {
            boards: {
                open: {
                    name: "Open",
                    expanded: true,
                    hideSettings: true,
                    data: [],
                    label: {
                        background: 'none',
                        color: 'black'
                    }
                },
                doing: {
                    name: "Doing",
                    expanded: true,
                    data: [],
                    label: {
                        background: "#6699cc",
                        color: 'white'
                    }
                },
                done: {
                    name: "Done",
                    expanded: true,
                    hideAddNewTask: true,
                    data: [],
                    label: {
                        background: "#00b140",
                        color: 'white'
                    }
                }
            }
        }
    },
    methods: {
        taskMoveHandler(data){
            console.log('Move between board handler: ', data);
        },
        changedHandler(data){
            console.log('Items were moved, data to change:', data);
        },
        clickedOnTask(data){
            console.log('Clicked on task', data);
        },
        listSettings(data) {
            let boardName = data.boardKey
            console.log(`Settings button clicked on ${boardName}`)
        },
        addNewTask(data) {
            let boardName = data.boardKey
            let newIndex = 0
            let fakeRequest = new Promise((resolve) => {
                let card = {
                    id: 123,
                    name: 'Test task',
                }
                setTimeout(()=>{
                    resolve(card)
                }, 500)
            })
            fakeRequest.then((result) => {
                console.log(`Adding '${result.name}' to '${boardName}' on index '${newIndex}'`)
                this.$refs.kanban.addNewCard(result, boardName, newIndex)
            })
            // addNewCard doesn't need 'ordering' field, it will be set automatically
        }
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
