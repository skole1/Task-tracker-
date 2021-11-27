<template>
    <button @click="onClick()" class="btn btn-primary">Add Task</button>
</template>

<script>
export default {
    name:'Button',
    props:{
        text:String,
        color:String
    },
    components:{
        
    },
    methods:{
        onClick(){
            this.$emit('toggle-add-task')
        }
    }
}
</script>