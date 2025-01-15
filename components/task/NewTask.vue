<template>
    <div>
        <div>
            <input class="btn btn bg-light-purple hover-shadow" type="button" value="New Task" @click="showAddModal()" />
        </div>
        <div v-if="showModal" class="position-fixed top-0 start-0 w-100 h-100 bg-dark bg-opacity-75 row align-items-center">
            <div class="modal-dialog form-group col-4">
                <div class="bg-white p-4 modal-content rounded">

                    <div class="text-center row modal-header">
                        <h3 class="col-11 modal-title text-black">New Task</h3>
                        <input class="btn-close col-1 text-xs" type="button" @click="showModal = false" />
                    </div>

                    <div class="modal-body">
                        <div class="row">
                            <div class="mt-3 col-6">
                                <label class="ml-2 mb-2 text-black" for="title">Title</label>
                                <input v-model="newTask.title" id="title" class="form-control" type="text" placeholder="My new task" />
                                <span class="text-sm ml-3 mt-2 text-danger"> {{ errors.title }} </span>
                            </div>
                            <div class="mt-3 col-6">
                                <label class="ml-2 mb-2 text-black" for="date">Date</label>
                                <input v-model="selected_date" id="date" class="form-control" type="date" @change="setDate()"/>
                                <span class="text-sm ml-3 mt-2 text-danger"> {{ errors.date }} </span>
                            </div>
                        </div>
                        <div class="mt-3 col-12">
                            <label class="ml-2 mb-2 text-black" for="description">Description</label>
                            <textarea v-model="newTask.description" id="description" class="form-control" type="" placeholder="This is my new task"></textarea>
                        </div>
                    </div>

                    <div class="mt-3 col-12 row justify-content-end modal-footer">
                        <div class="row col-10">
                            <input class="col-1 p-2 form-check-input" type="checkbox" name="done" id="done" v-model="newTask.status">
                            <label class="col-7 text-black" for="done">Mark as done</label>
                        </div>

                        <input id="save" class="col-2 btn bg-light-purple align-self-end" type="button" @click="processNewTask()" value="Save">
                    </div>

                </div>
            </div>
        </div>
    </div>
</template>

<script setup>

import { ref, reactive } from 'vue'

const props = defineProps(['taskCount'])
const emit = defineEmits(['newTask'])

// modal
let showModal = ref(false)
let selected_date = ref('')
let newTask = reactive({
    id: props.taskCount+1,
    title: '',
    status: false,
    date: '',
    description: ''
})
let errors = reactive({
    title: '',
    date: ''
})

const showAddModal = () => {
    errors.title = ''
    errors.date = ''
    selected_date.value = ''
    showModal.value = true
}

const setDate = () => {
    newTask.date = selected_date.value
}

const resetErrors = () => {
    errors.title = ''
    errors.date = ''
}

const processNewTask = () => {

    resetErrors()

    if(newTask.title == ''){
        errors.title = 'Title is required'
    }

    if(newTask.date == ''){
        errors.date = 'Date is required'
    }

    if(errors.title != '' || errors.date != ''){
        return
    }
    
    emit('newTask',newTask)

    showModal.value = false

    newTask = {
        id: props.taskCount+1,
        title: '',
        status: false,
        date: '',
        description: ''
    }
    
}

</script>