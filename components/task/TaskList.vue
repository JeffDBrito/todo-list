<template>
    <div class="row mb-5">
        <div class="col-md-9">
            <DateFilter @filterByDate="reloadTasks"/>
        </div>
        <div class="col-md-3">
            <TaskNewTask :taskCount="taskCount" @newTask="newTask"/>
        </div>

        <div class="col-md-12 mt-5">
            <div class="mt-5">
                <div class="mt-5 row">
                    <h3 class="col-10">Pending Tasks</h3>
                    <div class="col-1 row align-content-center justify-content-center">
                        <span class="rounded bg-dark-purple">{{ pending_tasks.length }}</span>
                    </div>
                    <input class="col-1 btn text-white text-sm" type="button" @click="togglePending()" :value="show_pending ? 'v' : '>'"/>
                </div>
                <hr>
                <div class="" v-if="show_pending">
                    <Task v-for="(task, index) in pending_tasks" :key="task.id" @updateTask="updateTask" @deleteTask="deleteTask" @toggleStatus="toggleStatus" :task="task" :index="index"/>
                </div>
                    
            </div>
            <div class="mt-5">
                <div class="mt-5 row">
                    <h3 class="col-10">Done Tasks</h3>
                    <div class="col-1 row align-content-center justify-content-center">
                        <span class="rounded bg-dark-purple">{{ done_tasks.length }}</span>
                    </div>
                    <input class="col-1 btn text-white text-sm" type="button" @click="toggleDone()" :value="show_done ? 'v' : '>'"/>
                </div>
                <hr>
                <div class="" v-if="show_done">
                    <Task v-for="(task, index) in done_tasks" :key="task.id" @deleteTask="deleteTask" @toggleStatus="toggleStatus" :task="task" :index="index"/>
                </div>
            </div>
        </div>

    </div>
</template>
<script setup>

import { ref, reactive } from 'vue'

const toast = useToast()

const today = new Date().toJSON().slice(0,10).replace(/-/g,'-');
let all_tasks = ref([])
let taskCount = ref(0)
let pending_tasks = ref([])
let done_tasks = ref([])
let show_pending = ref(true)
let show_done = ref(true)

let selected_date = ref('')

const togglePending = (show) => {
    show_pending.value = !show_pending.value
}

const toggleDone = (show) => {
    show_done.value = !show_done.value
}

const toggleStatus = (task) => {

    let title = (task.status ? 'Pending!' : 'Done!')
    let description = (task.status ? 'Task marked as pending.' : 'Task marked as done.')
    let color = (task.status ? 'warning' : 'success')
    
    showToast(title, description, color)

    reloadTasks()
}

const newTask = (task) => {
    let new_task = {
        id: task.id,
        title: task.title,
        status: task.status,
        date: task.date,
        description: task.description
    }

    showToast('Done!','Task created with success.','success')

    all_tasks.value.push(new_task)
    taskCount.value = taskCount.value + 1
    reloadTasks()
}

const reloadTasks = (date = null) => {

    if(!date || date == '' || date == 'all'){
        pending_tasks.value = all_tasks.value.filter(task => task.status === false)
        done_tasks.value = all_tasks.value.filter(task => task.status === true)
    }else if(date === 'today') {
        pending_tasks.value = all_tasks.value.filter(task => task.status === false && task.date === today)
        done_tasks.value = all_tasks.value.filter(task => task.status === true && task.date === today)
    }else{
        pending_tasks.value = all_tasks.value.filter(task => task.status === false && task.date === date)
        done_tasks.value = all_tasks.value.filter(task => task.status === true && task.date === date)
    }


}

const deleteTask = (task) => {
    all_tasks.value = all_tasks.value.filter(t => t !== task)

    showToast('Done!','Task deleted!','error')
    
    reloadTasks()
}

const updateTask = (details) => {

    let task = details[0]
    let updated_task = details[1]

    task.title = updated_task.title
    task.status = updated_task.status
    task.date = updated_task.date
    task.description = updated_task.description

    let title = 'Done!'
    let description = 'Task updated with success.'
    let color = 'success'

    showToast(title, description, color)

    reloadTasks()
}

const showToast = (title, description, color) => {
    toast.clear()
    toast.add({
        title: title,
        description: description,
        color: color
    })
}


</script>