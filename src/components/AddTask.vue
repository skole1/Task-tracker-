<template>
    <form @submit="onSubmit" class="add-task">
        <div class="">
            <label for="Task">Task</label>
            <input type="text" v-model="text" name="text" class="form-control" placeholder="Task">
        </div>
        <div class="">
            <label for="Day & Time">Day & Time</label>
            <input type="text" v-model="day" name="day" class="form-control" placeholder="Day & Time">
        </div>
        <div class="reminder">
            <label for="Set Reminder">Set Reminder</label>
            <input type="checkbox" v-model="reminder" name="reminder" class="form-control-check">
        </div>
        <div class="mb-3">
            <input type="submit" value="Set Task" class="form-control btn btn-primary btn-block">
        </div> 
    </form>
</template>

<script>
export default { 
    name:'AddTask',
    data(){
        return{
            text:'',
            day:'',
            reminder: false
        }
    },
    methods:{
        onSubmit(e){
            e.preventDefault()

            if(!this.text){
                alert('Please add a tasks')
                return
            }

            const newTask = {
                id: Math.floor(Math.random() * 100000),
                text:this.text,
                day: this.day,
                reminder:this.reminder
            }

            this.$emit('add-task',newTask)

            this.text = ''
            this.day = ''
            this.reminder = false




        }
    }
}
</script>

<style scoped>
.reminder {
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>