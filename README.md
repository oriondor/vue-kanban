# Usage
```
<v-kaban v-if="boards"
    host="http://localhost:8081"
    path="/kanban"
    v-model="boards"
></v-kaban>
```
- define a host and path from which kanban will be fetched
- you will need to define `v-model="boards"` where `boards` is a structure that looks like this:
```
boards: {
    open: {
        name: "Open", // HTML name of the board
        expanded: true, // defines whether board is collapsed or expanded
        hideSettings: true, // you can pass config that will hide the settings button on each individual board
        data: [], // Leave it empty to let kanban lazy-load data for you
        label: { // CSS settings for the label
            background: 'none',
            color: 'black'
        }
    },
    doing: {
        name: "Doing",
        expanded: false, // If value is false, data will not be loaded until expanded
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
```
- note that `data: []` defines an empty array here because data should be fetched after inital
kanban load, from the endpoint you specified in previous v-kaban props

#### ! IMPORTANT 
It's good to store the above configuration somewhere in your database

### There are some optional props you'd like to pass to the kanban
```
queryBoardName: {
    type: String,
    default: "status" – defines query paramenter from which specific board is to be resolved
},
pathTemplate: {
    type: String,
    default: "{path}?{queryBoardName}={board}" – defines an url pattern by which boards are to be resolved, {board} name is taken from above boards configuration
},
filters: {
    type: Object,
    default: Object – you can pass additional query params to your endpoint
},
tasksCounterLabel: {
    type: String,
    default: '{count} Issue(s)' – template for the label when you hover over tasks stack icon on each board
},
addNewTaskLabel: {
    type: String,
    default: 'New task' – label when you hover over add new task button
},
listSettingsLabel: {
    type: String,
    default: 'Settings' – label when you hover over settings button
},
```
