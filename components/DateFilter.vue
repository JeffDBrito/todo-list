<template>
    <div class="row">
        <div class="row col-md-4">
            <input class="mx-2 col-md-5 btn bg-dark-purple" type="button" value="All" @click="$emit('filterByDate','all')"/>
            <input class="mx-2 col-md-5 btn bg-dark-purple" type="button" value="Today" @click="$emit('filterByDate','today')" />
        </div>
        <div class="col-md-8">
            <input class="form-control" type="date" v-model="selected_date" @change="$emit('filterByDate',getSelectedDate())" />
        </div>
    </div>
</template>

<script setup>

import { ref } from 'vue'

defineEmits(['filterByDate'])

const selected_date = ref('')

const getSelectedDate = () => { 

    return selected_date.value

}

</script>