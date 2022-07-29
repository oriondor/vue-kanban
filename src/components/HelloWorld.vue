<template>
    <div class="hello">
        <v-kaban v-if="boards"
            host="http://localhost:8081"
            path="/kanban"
            v-model="boards"
        ></v-kaban>
    </div>
</template>

<script>
import VKaban from "@/components/vKaban";

export default {
    name: 'HelloWorld',
    components: {VKaban},
    props: {
        msg: String
    },
    data(){
        return {
            boards: {
                open: {
                    name: "Open",
                    expanded: true,
                    label: {
                        background: 'none',
                        color: 'black'
                    }
                },
                doing: {
                    name: "Doing",
                    expanded: true,
                    label: {
                        background: "#6699cc",
                        color: 'white'
                    }
                },
                done: {
                    name: "Done",
                    expanded: true,
                    label: {
                        background: "#00b140",
                        color: 'white'
                    }
                }
            }
        }
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
    margin: 40px 0 0;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: inline-block;
    margin: 0 10px;
}

a {
    color: #42b983;
}
</style>
