<template>
    <div class="row">
        <div class="col-md-2">
            <input class="form-control btn btn-primary" type="button" value="all" @click="filterByDate('all')"/>
        </div>
        <div class="col-md-2">
            <input class="form-control btn btn-primary" type="button" value="today" @click="filterByDate('today')" />
        </div>
        <div class="col-md-8">
            <input class="form-control" type="date" v-model="selected_date" @change="filterByDate('date')" />
        </div>
    </div>
</template>

<script setup>

import { ref } from 'vue'

const selected_date = ref('')

const filterByDate = (date = null) => { 

    if(date === 'date'){
        console.log(selected_date.value)
    }else{
        console.log(date)
    }

}

</script>