<template>

    <div>
        <div :class="(index%2 == 0 ? 'task-bg-primary' : 'task-bg-secondary' )+' my-2 py-3 px-5 row align-content-center justify-content-center '+ (task.status ? 'task-done' : '')">
            <div class="col-1 align-content-center">
                <input class="rounded form-check-input" v-model="task.status" type="checkbox" @change="$emit('toggleStatus',task)" />
            </div>
            <div class="col-8 align-content-center">
                <span class="text-sm">{{ formatDate(task.date) }}</span>
                <p :class="'p-0 m-0 text-md fw-bold fs-5 '+(task.status ? 'text-decoration-line-through' : '')"> {{ task.title }}</p>
            </div>
            <div class="row col-3 align-content-center">
                <a class="col-4 btn text-white" @click="toggleEditModal()" href="#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil-square" viewBox="0 0 16 16"><path d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z"/><path fill-rule="evenodd" d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5z"/></svg>
                </a>
                <a class="col-4 btn text-white" @click="$emit('deleteTask',task)" href="#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16"><path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0z"/><path d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4zM2.5 3h11V2h-11z"/></svg>
                </a>
                <input class="col-4 btn text-white text-sm" type="button" @click="toggleDescription()" :value="show_description ? 'v' : '>'"/>
            </div>
            <div class="col-12 mt-5" v-if="show_description">
                <p class="text-md">{{ task.description }}</p>
            </div>
        </div>

        <div v-if="show_edit_modal" class="position-fixed top-0 start-0 w-100 h-100 bg-dark bg-opacity-75 row align-items-center">
            <div class="modal-dialog form-group col-4">
                <div class="bg-white p-4 modal-content rounded">

                    <div class="text-center row modal-header">
                        <h3 class="col-11 modal-title text-black">Edit Task</h3>
                        <input class="btn-close col-1 text-xs" type="button" @click="show_edit_modal = false" />
                    </div>

                    <div class="modal-body">
                        <div class="row">
                            <div class="mt-3 col-6">
                                <label class="ml-5" for="title">Title</label>
                                <input v-model="updated_task.title" id="title" class="form-control" type="text" placeholder="My new task" />
                            </div>
                            <div class="mt-3 col-6">
                                <label class="ml-5" for="date">Date</label>
                                <input v-model="updated_task.date" id="date" class="form-control" type="date" @change="setDate()"/>
                            </div>
                        </div>
                        <div class="mt-3 col-12">
                            <label class="ml-5" for="description">Description</label>
                            <textarea v-model="updated_task.description" id="description" class="form-control" type="" placeholder="This is my new task"></textarea>
                        </div>
                    </div>

                    <div class="mt-3 col-12 row justify-content-end modal-footer">
                        <div class="row col-10">
                            <input class="col-1 p-2 form-check-input" type="checkbox" name="done" id="done" v-model="updated_task.status">
                            <label class="col-7 text-black" for="done">Mark as done</label>
                        </div>

                        <input id="save" class="col-2 btn bg-light-purple align-self-end" type="button" @click="updateTask()" value="Save">
                    </div>

                </div>
            </div>
        </div>

    </div>

</template>
<script setup>

import { ref, reactive } from 'vue';

const toast = useToast()

const props = defineProps(['task','index'])

const emit = defineEmits(['toggleStatus','updateTask','deleteTask' ])

let show_description = ref(false)
let show_edit_modal = ref(false)
let updated_task = ref({
    title: props.task.title,
    status: props.task.status,
    date: props.task.date,
    description: props.task.description
})
let selected_date = ref('')

const toggleDescription = () => {
    show_description.value = !show_description.value
}

const toggleEditModal = () => {
    show_edit_modal.value = !show_edit_modal.value
}

const updateTask = () => {
    emit('updateTask',[props.task,updated_task.value])
    show_edit_modal.value = false
}

const formatDate = (date) => {
    return new Date(date).toJSON().slice(0,10).replace(/-/g,'/')
}

</script>