<template>
    <div class="flex flex-col">
        <h3 class="text-2xl text-blue-500">Clients List</h3>
        <p>{{name}}</p>
        <p>{{location}}</p>
    </div>
</template>

<script>
import { reactive } from 'vue'
export default {
    setup () {
        const state=reactive({
            name:'Njuguna',
            location:' Nairobi Kenya'
    })

        return {
            ...state
        }
    }
}
</script>

<style lang="scss" scoped>

</style>