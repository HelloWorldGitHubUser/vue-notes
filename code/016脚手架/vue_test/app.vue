<template>
    <div>
        <Student/>
        <school/>   
    </div>
</template>


<script>
import School from './School.vue';
import Student from './Student.vue';
export default {
    name: 'APP',
    components:{
        Student,
        School
    }
};
</script>
