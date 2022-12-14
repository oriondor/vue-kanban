# Usage
```
<v-kaban v-if="boards"
    host="http://localhost:8081"
    path="/kanban"
    v-model="boards"
></v-kaban>

<!-- 
It is possible to define an ordering method for kanban
This component works with 2 possible ordering methods:
linkedList
stringSort
default is 'none' (NOTE that in this case @changedElements is not triggered)
read below for the detailed description on these methods
<v-kaban ...
    ordering-type="linkedList"
    @changedElements="changedHandler"
    ...
>
</v-kaban>
-->
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
- Note that `data: []` defines an empty array here because data should be fetched after inital
kanban load, from the endpoint you specified in previous v-kaban props
- Note that it's ok to store the above configuration somewhere in your database

## There are some optional props you'd like to pass to the kanban
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
useCustomCardStyle: {
    type: Boolean,
    default: false – pass true and overwrite card-view slot with your own HTML
}
```

## Order handling

It is useful to keep your kanban ordered in the way you want
so the couple of functions weer implemented for this

###### NOTE that these functions are just to give instructions for a backend

Assuming you have some model that you use as a card/task in the kanban.
If it is the case, then you need to extend this model with an extra field called `ordering`.

Component has default implementation for two ordering methods:

<details>
<summary>linkedList (recommended for 99% of cases)</summary>

### Linked list is nothing more than a list with the items having links to the next item and a flag that tells whether element is on the top

The structure of the list is as follows:
```
[
    ...,
    {
        id: <int>,
        ....,
        ordering: {
            is_head: <bool>,
            next: <int>
        },
        ....
    },
    ...,
]
```
#### Please note that actual sorting should be done on the backend 
Component is only responsible for correct setting of a given properties and emitting an array
with changed elements via event `@changedElements`

It is your responsibility to save it on the backend side and output in the correct order on page load.
Otherwise, it will lead to an unexpected behaviour.
</details>

<details>
<summary>stringSort</summary>

### String sort seems to be quite hacky solution ;)

However, the idea behind is to reduce pressure for the backend by providing strings with numbers as
ordering entries. 

Let's say you have a lot of different boards and all you do is dragging them only from left to right.
In case of `linkedList` you'd usually have to update 2-3 elements while with `stringSort` you'd only need
to update 2 elements maximum and more frequently only 1 element!
Additionally, all you need to update in this case is a string field and not the whole object.
So, if such a speedup is desired in your system, 
then this solution is for you :)

The structure of the item in this case is as follows
```
[
    ...,
    {
        id: <int>,
        ....,
        ordering: <string>,
        ....
    },
    ...,
]
```

On the backend sorting for this can be done by using simple `ORDER BY ordering ASC` statement

##### How does it work???

Each element of the given sequence has its string that describes the position 
of the element inside the board. When adding element between other two elements, it receives the 
ordering string value as a string concat of the surrounding elements. For instance putting an element 
between 1 and 2 will produce 12 ordering string for this element.


###### Please note that this method is more efficient in terms of updating speed, ....
However, dragging big amount of elements inside one board can lead to really large strings produced by this method.
Therefore, use it on the boards that do not require frequent swap inside
one board. Otherwise, use stable linkedList sorting feature.
</details>


## Lazy load handling

Lazy loading implemented to be used with 2 kinds of loading. Depending on your backend or preferences you can switch between this two type.

##### First and default type is `perQuantity`

This means that endpoint for fetching your data will look like `...&from=0&to=7`

##### Other type is `perPage`

This type is used to work with pagination frameworks well. 

###### Each time you scroll to the bottom of the board kanban will send a request to a backend and update props depending on your lazy method

There are some props in the component that can help to overrule lazy load behavior 
```
lazy: {
    type: Boolean,
    default: true
},
lazyNumberPerTime: {
    type: Number,
    default: 7
},
lazySplitType: {
    type: String,
    default: 'perQuantity'
},
lazyFilterProps: {
    type: Array,
    default: () => ['from', 'to']
}
```
When you use `lazySplitType='perPage'` you'd need to overrule `lazyFilterProps` to `['page']` or wherever name
you want to use
