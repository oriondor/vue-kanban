<template>
    <div class="v-kaban-boards"
    >
        <template :key="bKey" v-for="[bKey, bConfig] in Object.entries(boards)">
            <div class="v-kaban-board" :class="{collapsed: !bConfig.expanded}">
                <div class="v-kaban-board-header">
                    <div class="v-kaban-collapse"
                         @click="toggleCollapse(bKey)"
                    >
                        <collapse-chevron
                            :rotate="bConfig.expanded"
                        ></collapse-chevron>
                    </div>
                    <div class="v-kaban-label-wrap">
                        <div class="v-kaban-label" :style="bConfig.label">
                            <div>{{ bConfig.name }}</div>
                        </div>
                    </div>
                </div>
                <div class="v-kaban-board-body"></div>
            </div>
        </template>
    </div>
</template>

<script>
import axios from "axios";
import CollapseChevron from "@/components/collapseChevron";

export default {
    name: "vKaban",
    components: {CollapseChevron},
    props: {
        host: {
            type: String,
        },
        path: {
            type: String,
        },
        modelValue: {
            types: Object
        },
        queryBoardName: {
            type: String,
            default: "status"
        },
        pathTemplate: {
            type: String,
            default: "{path}?{queryBoardName}={board}"
        },
        filters: {
            type: Object,
            default: Object
        }
    },
    data() {
        return {}
    },
    methods: {
        getBoardEndpoint(board) {
            let template = {
                'queryBoardName': this.queryBoardName,
                'path': this.path,
                'board': board,
            }
            return this.host + this.pathTemplate.replace(/{(.+?)}/g, (_, g1) => template[g1] || g1)
        },
        retrieveBoardData(board) {
            return axios.get(this.getBoardEndpoint(board)).then((result) => {
                return result.data
            })
        },
        toggleCollapse(bKey) {
            this.boards[bKey].expanded = !this.boards[bKey].expanded
        },
    },
    mounted() {
        // this.retrieveBoardData('open').then((result)=>{
        //     console.log(result);
        // })
        console.log(this.modelValue);
    },
    computed: {
        boards: {
            get() {
                return this.modelValue
            },
            set(data) {
                this.$emit('update:modelValue', data);
            }
        }
    }
}
</script>

<style scoped>
.v-kaban-boards {
    overflow-x: auto;
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    height: 90vh;
}

.v-kaban-board {
    border-radius: 5px;
    padding: 5px;
    min-width: 450px;
    margin: 0 10px 0 10px;
    background: #f0f0f0;
    height: 100%;
}

.collapsed {
    max-width: 50px;
    min-width: unset;
}

.v-kaban-board-header {
    display: flex;
    flex-wrap: nowrap;
    flex-direction: row;
}

.collapsed .v-kaban-board-header {
    flex-direction: column;
}

.v-kaban-board-header > div {
    padding: 4px 8px 4px 8px;
}

.v-kaban-collapse {
    min-width: 15px;
    min-height: 20px;
    cursor: pointer;
    border-radius: 5px;
    white-space: nowrap;
    display: inline-block;
}
.v-kaban-collapse:hover{
    background: #c5c5c5;
}

.v-kaban-label {
    font-weight: 700;
    font-size: 10pt;
    height: 20px;
    border-radius: 13px;
    padding: 2px 8px 1px 8px;
    white-space: nowrap;
}

.collapsed .v-kaban-label-wrap {
    transform: rotate(90deg);
    margin-top: 10px;
}

.collapsed .v-kaban-label {
    white-space: nowrap;
    display: inline-block;
}

.v-kaban-board-body {

}
</style>