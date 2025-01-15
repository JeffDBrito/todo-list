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
                                <label class="ml-5" for="title">Title</label>
                                <input v-model="newTask.title" id="title" class="form-control" type="text" placeholder="My new task" />
                            </div>
                            <div class="mt-3 col-6">
                                <label class="ml-5" for="date">Date</label>
                                <input v-model="selected_date" id="date" class="form-control" type="date" @change="setDate()"/>
                            </div>
                        </div>
                        <div class="mt-3 col-12">
                            <label class="ml-5" for="description">Description</label>
                            <textarea v-model="newTask.description" id="description" class="form-control" type="" placeholder="This is my new task"></textarea>
                        </div>
                    </div>

                    <div class="mt-3 col-12 row justify-content-end modal-footer">
                        <div class="row col-10">
                            <input class="col-1 p-2 form-check-input" type="checkbox" name="completed" id="completed" v-model="newTask.status">
                            <label class="col-7 text-black" for="completed">Mark as completed</label>
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

const showAddModal = () => {
    showModal.value = true
}

function setDate(){
    newTask.date = selected_date.value
}

function processNewTask(){
    
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