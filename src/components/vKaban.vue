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
                    <label-badge
                        :text="bConfig.name"
                        :style="bConfig.label"
                        :rotate="bConfig.expanded"
                    ></label-badge>
                    <div style="flex-grow: 1;"></div>
                    <div class="v-kaban-header-right-bar">
                        <div class="v-kaban-hrb-count">
                            <img :alt="tasksCounterLabelGet(bConfig.data.length)" :title="tasksCounterLabelGet(bConfig.data.length)" src="data:image/png;base64,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">
                        </div>
                        <span style="padding: 2px"> {{bConfig.data.length}}</span>
                        <div class="v-kaban-hrb-buttons">
                            <div v-if="!bConfig.hideAddNewTask" class="round-hoverable v-kaban-hrbb-add-task" @click="clickAddNewTask(bKey, bConfig)">
                                <img :alt="addNewTaskLabel" :title="addNewTaskLabel" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAQAAAAEACAYAAABccqhmAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAaqSURBVHhe7d3bjt11HcbhKciB4o1YOTCK3gC4OTDgBajoDQDFIAheg5iAwXAN2BN3p1orEa0RqYq7q6hGATX6fieNaZNfSGnn37V5nyf5JM2ambUmmfm9qzNrzcwJNe5JD6Zn0/fTm+na9a5ev+wb6ZNpXhc4Avenx9Pv0n9vsd+nJ9K8LXCgHkm/SatDfiu9keY6gAPzTFod6ttpvmwADsQLaXWQ76RvJ2DPXUirA3wWPZWAPfVQejetDu9ZNNf9cAL2zAfS62l1cM+yX6a5LWCPfDmtDuwWzW0Be+Jc+nlaHdYtei3NbQJ74CPp7bQ6rFs0t3U+AXtgnrW3OqhbNo82AHvg5bQ6pFv2SgL2wKtpdUi37GICdmy+GXcprQ7pll1OfmoQdmwek/9VWh3SLbuS7kvADs0AzJNzVod0y2Z0DADsmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgDKvZ5Wh3TLZnTOJUrNB3/ufbTbPpjm3nh1SLfsSvpQWr1PurttPsRzA+fTk+k76dV0Kc0n3twTaDfNPf98DP6WVod0y/6e5rbnfVi9b7o7zcdgzuKcyTmbF9JH05mNwhfTT9M/0uoTQdJ+NWd1RuFL6baH4KH0k7S6AUmH0dx5P5zelyfSO2l1hZIOq3fTfGlwS76VVlci6bB7Ib2nZ9LqDSUdR8+mpc+nf6fVG0k6juaMP5JuMo8lv5VWbyDpuPpjmudw/N/jafWKko6z+Ub/qXvTn9LqlSQdZ39O8yzCk49dv0BSV59Ip98VXL1Q0nH3XDr54Q0XSOrpR+nkzRsukNTTnP2TazdcIKmnOfsGQCrtdACu3nCBpJ7m7J9+I2D1QknH3Y/T6UMBqxdKOu6eTycfv+ECST09mE6fDviX6xdI6uiv6fSpwGN+MGD1SpKOs5t+Q9D8aKAfB5Y6mh/+uz/dZH5JgF8IIh13/0mPpiWPCEjH3el3/t/Li2n1hpIOu5fSLXk6+XJAOo7mv/1fT+/LZ9PP0uoKJR1Gl9Pn0m25Jz2W5s8M/TOtbkDSfjVnde68v5LmV/7dsRmCB9JT6eV0Mc2yzF+InT9OqN316zR/qHP1ibBlc5tz26v3SXevOYNzFudMztmcMzpndc7spuYG7tPOm+dwzCfC6pBu2XzizWPJq/dJd7fNDzv7bf5E9+qQbtn8aWpgx+b53HMYV4d0y+Z/HXPvA+yQAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIoZAChmAKCYAYBiBgCKGQAoZgCgmAGAYgYAihkAKGYAoJgBgGIGAIrNAMxhXB3SLbuSDADs2Ll0Ka0O6ZZdTvckYMe+l1aHdMsuJmAPfDetDumWvZKAPXAhrQ7pln0tAXvgfHo7rQ7qFr2THkjAHphvBL6WVod1i36R5jaBPfFYWh3WLfpqAvbIPCZ/N54P4PF/2FOfSf9Kq4N7Fs11fzoBe+rptDq8Z9FcN7DnXkyrA3wnvZSAA/FcWh3k2+mbCTgwj6Y30upQ30q/TV9IwIH6cHoy/SGtDvmqt9I8u3DeFjgC96ZPpfnS4Afparp2vfn3XPZ8mteZHzHm6J2c/A9zMhykCTMyOgAAAABJRU5ErkJggg==">
                            </div>
                            <div v-if="!bConfig.hideSettings" class="round-hoverable v-kaban-hrbb-settings" @click="clickSettings(bKey, bConfig)">
                                <img :alt="listSettingsLabel" :title="listSettingsLabel" src="data:image/png;base64,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">
                            </div>
                        </div>
                    </div>
                </div>
                <div class="v-kaban-board-body" v-show="bConfig.expanded">
                    <div :key="card.id" v-for="card in bConfig.data">
                        <template v-if="useCustomCardStyle">
                            <slot name="card-view" v-bind="card"></slot>
                        </template>
                        <template v-else>
                            <kaban-card
                                :card="card"
                            ></kaban-card>
                        </template>
                    </div>
                </div>
            </div>
        </template>
    </div>
</template>

<script>
import axios from "axios";
import CollapseChevron from "@/components/collapseChevron";
import LabelBadge from "@/components/labelBadge";
import KabanCard from "@/components/kabanCard";

export default {
    name: "vKaban",
    components: {KabanCard, LabelBadge, CollapseChevron},
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
        },
        tasksCounterLabel: {
            type: String,
            default: '{count} Issue(s)'
        },
        addNewTaskLabel: {
            type: String,
            default: 'New task'
        },
        listSettingsLabel: {
            type: String,
            default: 'Settings'
        },
        useCustomCardStyle: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {}
    },
    methods: {
        initListeners(){
            let bodyDivs = document.getElementsByClassName("v-kaban-board-body")
            for (let bd of bodyDivs) {
                bd.addEventListener('mousedown', this.mouseDownHandler, false)
            }
        },
        mouseDownHandler(event){
            let self = this
            let mainIndex = null
            for (let index in event.path) {
                if (event.path[index].className === 'v-kaban-board-body') {
                    mainIndex = index - 1
                    break
                }
            }
            if (mainIndex === null || mainIndex < 0) {
                return
            }
            let draggable = event.path[mainIndex]

            let mouseMoveHandler = function(e){
                draggable.classList.add("dragging");
                draggable.style.pointerEvents = 'none';
                document.body.style.userSelect = 'none';
                let deviation = draggable.initialDeviation
                draggable.style.left = e.clientX - deviation.x + 'px'
                draggable.style.top = e.clientY + 'px'
                // console.log('left', event.currentTarget.style.left);
                // console.log('top', event.currentTarget.style.top);
                let bodyDivs = document.getElementsByClassName("v-kaban-board-body")
                for (let bd of bodyDivs) {
                    bd.addEventListener('mouseover', mouseOverHandler, false)
                    bd.addEventListener('mouseleave', mouseLeaveHandler, false)
                }
            }

            let mouseUpHandler = function(e){
                console.log('up', e);
                draggable.classList.remove("dragging");
                document.body.removeEventListener('mousemove', mouseMoveHandler)
                document.body.removeEventListener('mouseup', mouseUpHandler)
                document.body.style.userSelect = 'initial';
                draggable.style.pointerEvents = 'all';
                let bodyDivs = document.getElementsByClassName("v-kaban-board-body")
                for (let bd of bodyDivs) {
                    bd.removeEventListener('mouseover', mouseOverHandler)
                    bd.removeEventListener('mouseleave', mouseLeaveHandler)
                }
            }

            let mouseOverHandler = function(e) {
                console.log('over', e);
                self.shadowElement(e, draggable)
            }

            let mouseLeaveHandler = function(e) {
                console.log('leave', e);
                self.shadowElement(e, null, 'remove')
            }

            let br = draggable.getBoundingClientRect()
            draggable.initialDeviation = {x: event.clientX - br.x, y: event.clientY - br.y}
            document.body.addEventListener('mousemove', mouseMoveHandler, false)
            document.body.addEventListener('mouseup', mouseUpHandler, false)

        },
        shadowElement(e, base=null, method='add'){
            let place = null
            console.log('place path', e.path);
            for (let index in e.path) {
                if (e.path[index].className === 'v-kaban-board-body') {
                    place = e.path[index]
                    break
                }
            }
            if (!place) {
                return
            }
            if (method === 'add' && base !== null) {
                console.log('base', base);
                let transparentDraggable = base.cloneNode(true)
                console.log('cloned node', transparentDraggable);
                transparentDraggable.style.opacity = '0.5'
                transparentDraggable.classList.remove("dragging")
                place.appendChild(transparentDraggable)
            } else if (method === 'remove') {
                let children = place.childNodes
                for (let child of children) {
                    if (child.style && child.style.opacity === '0.5') {
                        place.removeChild(child)
                    }
                }
            }
        },
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
        getBoards(){
            for (let b of Object.keys(this.boards)) {
                this.retrieveBoardData(b).then((data)=>{
                    this.boards[b].data = data
                })
            }
        },
        toggleCollapse(bKey) {
            this.boards[bKey].expanded = !this.boards[bKey].expanded
        },
        tasksCounterLabelGet(count){
            let template = {
                'count': count,
            }
            return this.tasksCounterLabel.replace(/{(.+?)}/g, (_, g1) => template[g1])
        },
        clickAddNewTask(bKey, bConfig){
            this.$emit('addNewTask', {
                boardKey: bKey,
                boardConfig: bConfig,
            })
        },
        clickSettings(bKey, bConfig){
            this.$emit('settings', {
                boardKey: bKey,
                boardConfig: bConfig,
            })
        },
    },
    mounted() {
        this.getBoards()
        this.initListeners()
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
    overflow-y: hidden;
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    height: 80vh;
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
    max-width: 35px;
    min-width: unset;
}

.v-kaban-board-header {
    display: flex;
    flex-wrap: nowrap;
    flex-direction: row;
    vertical-align: central;
    align-items: center;
    /*justify-content: flex-end;*/
}

.collapsed .v-kaban-board-header {
    flex-direction: column;
}

.v-kaban-board-header > div > div {
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

.collapsed .v-kaban-header-right-bar {
    flex-direction: column;
}

.v-kaban-header-right-bar{
    align-items: center;
    display: flex;
    justify-content: flex-end;
}

.v-kaban-header-right-bar img{
    width: 18px;
    height: 18px;
}

.v-kaban-hrb-buttons {
    display: flex;
    justify-content: space-between;
}
.round-hoverable {
    padding: 3px!important;
    border-radius: 3px;
    cursor: pointer;
}
.round-hoverable:hover{
    background: #c5c5c5;
}

.collapsed .v-kaban-hrb-buttons {
    flex-direction: column;
}

.v-kaban-board-body {
    height: 100%;
    overflow-y: auto;
}

.v-kaban-board-body div {
    cursor: pointer;
    min-width: 445px;
    min-height: 100px;
}
.dragging{
    position: absolute;
}
</style>