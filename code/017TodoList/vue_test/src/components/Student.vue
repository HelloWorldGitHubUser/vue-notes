<template>
    <div>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生年龄：{{age}}</h2>
    </div>
</template>

<script>
import {show} from '../mixin'
export default {
    name: 'Student',
    data(){
        return{
            name:'halo',
            age:18
        }
    },
    mixins:[show]
};
</script>