<template>
    <div class="row mb-5">
        <div class="col-md-9">
            <DateFilter @filterByDate="filterByDate"/>
        </div>
        <div class="col-md-3">
            <TaskNewTask :taskCount="taskCount" @newTask="newTask"/>
        </div>

        <div class="col-md-12 mt-5">
            <div class="mt-5">
                <div class="mt-5 row">
                    <h3 class="col-11">Uncompleted Tasks</h3>
                    <input class="col-1 btn text-white text-sm" type="button" @click="toggleUncompleted()" :value="show_uncompleted ? 'v' : '>'"/>
                </div>
                <hr>
                <div class="" v-if="show_uncompleted">
                    <Task v-for="(task, index) in uncompleted_tasks" :key="task.id" @deleteTask="deleteTask" @toggleStatus="updateTasks" :task="task" :index="index"/>
                </div>
                    
            </div>
            <div class="mt-5">
                <div class="mt-5 row">
                    <h3 class="col-11">Completed Tasks</h3>
                    <input class="col-1 btn text-white text-sm" type="button" @click="toggleCompleted()" :value="show_completed ? 'v' : '>'"/>
                </div>
                <hr>
                <div class="" v-if="show_completed">
                    <Task v-for="(task, index) in completed_tasks" :key="task.id" @deleteTask="deleteTask" @toggleStatus="updateTasks" :task="task" :index="index"/>
                </div>
            </div>
        </div>

    </div>
</template>
<script setup>

import { ref, reactive } from 'vue'

const toast = useToast()

const today = new Date()
let all_tasks = ref([])
let taskCount = ref(0)
let uncompleted_tasks = ref([])
let completed_tasks = ref([])
let show_uncompleted = ref(true)
let show_completed = ref(true)

let selected_date = ref('')

const filterByDate = (date = null) => { 
    console.log(date)
}

const toggleUncompleted = (show) => {
    show_uncompleted.value = !show_uncompleted.value
}

const toggleCompleted = (show) => {
    show_completed.value = !show_completed.value
}

const newTask = (task) => {
    let new_task = {
        id: task.id,
        title: task.title,
        status: task.status,
        date: task.date,
        description: task.description
    }

    toast.add({
        title: 'Done!',
        description: 'Task created with success.',
        color: 'success'
    })

    all_tasks.value.push(new_task)
    taskCount.value = taskCount.value + 1
    updateTasks()
}

const updateTasks = () => {
    uncompleted_tasks.value = all_tasks.value.filter(task => task.status === false)
    completed_tasks.value = all_tasks.value.filter(task => task.status === true)
}

const deleteTask = (task) => {
    all_tasks.value = all_tasks.value.filter(t => t !== task)
    updateTasks()
}


</script>