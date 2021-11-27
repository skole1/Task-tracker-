<template>
    <header class="row justify-content-center">
        <div class="col-md-3">
            <h1>{{ title }}</h1>
        </div>
        <div class="col-md-3">
            <Button @toggle-add-task="$emit('toggle-add-task')" />
        </div>
    </header>
</template>

<script>
import Button from './Button.vue'
export default {
    name:'Header',
    props:['title'],
    components:{
        Button
    }
}
</script>

<style scoped>

</style>