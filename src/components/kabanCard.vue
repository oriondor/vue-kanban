<template>
    <div class="v-kaban-card">
        <div class="v-kaban-task-title" @click="clickTask(card.id)">
            {{ card.name }}
        </div>
        <div class="v-kaban-task-middle">
            <div class="v-kaban-task-labels" v-if="card.labels">
                <div class="v-kaban-task-label" v-for="label in card.labels" :key="label">
                    {{label}}
                </div>
            </div>
        </div>
        <div class="v-kaban-task-bottom">
            <div class="v-kaban-task-id">
                #{{ card.id }}
            </div>
            <div class="v-kaban-task-assignees" v-if="card.assignees">
                <div class="v-kaban-task-assignee" v-for="assignee in card.assignees" :key="assignee.id">
                    <span v-for="part in assignee.name.split(' ')" :key="part">
                        {{ part[0] }}
                    </span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "kabanCard",
    props: {
        card: {
            type: Object,
            default: Object,
        }
    },
    data(){
        return {

        }
    },
    methods: {
        clickTask(id) {
            this.$emit('taskClicked', {
                id: id
            })
        }
    },
}
</script>

<style scoped>

.v-kaban-card {
    width: 96%;
    min-height: 70px;
    background: #ffffff;
    border-radius: 6px;
    margin: 6px auto 6px auto;
    padding: 7px 0 4px 8px;
}

.v-kaban-task-title {
    width: 100%;
    text-align: justify;
    cursor: pointer;
}

.v-kaban-task-title:hover {
    text-decoration: underline;
}

.v-kaban-task-middle {
    display: flex;
    flex-wrap: wrap;
    min-height: 40px;
}

.v-kaban-task-bottom {
    display: flex;
}
.v-kaban-task-id {
    flex-grow: 0;
}

.v-kaban-task-assignees {
    flex-grow: 2;
    justify-content: flex-end;
    display: flex;
    margin-right: 7px;
}
.v-kaban-task-assignee {
    margin: 2px;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 25px;
    height: 25px;
    font-size: 9pt;
    border-radius: 50%;
    background: #e7e7e7;
}

.v-kaban-task-labels {
    display: flex;
}
</style>