<template>
    <div>
        <div>
            <input class="btn btn-success" type="button" value="New Task" @click="showAddModal()" />
        </div>
        <div v-if="showModal" class="position-fixed top-0 start-0 w-100 h-100 bg-dark bg-opacity-75 row align-items-center">
            <div class="modal-dialog form-group col-4">
                <div class="bg-white p-4 modal-content rounded">

                    <div class="text-center row modal-header">
                        <h3 class="col-11 modal-title">New Task</h3>
                        <input class="btn-close col-1 text-xs" type="button" @click="showModal = false" />
                    </div>

                    <div class="modal-body">
                        <div class="row">
                            <div class="mt-3 col-6">
                                <label class="ml-5" for="title">Title</label>
                                <input id="title" class="form-control" type="text" placeholder="My new task" />
                            </div>
                            <div class="mt-3 col-6">
                                <label class="ml-5" for="date">Date</label>
                                <input id="date" class="form-control" type="date" value="Date" />
                            </div>
                        </div>
                        <div class="mt-3 col-12">
                            <label class="ml-5" for="description">Description</label>
                            <textarea id="description" class="form-control" type="" placeholder="This is my new task"></textarea>
                        </div>
                    </div>

                    <div class="mt-3 col-12 row justify-content-end modal-footer">
                        <input id="save" class="col-2 btn btn-success align-self-end" type="button" value="Save">
                    </div>

                </div>
            </div>
        </div>
    </div>
</template>

<script setup>

import { ref } from 'vue'


// modal
let showModal = ref(false)

const showAddModal = () => {
    showModal.value = true
}




</script>