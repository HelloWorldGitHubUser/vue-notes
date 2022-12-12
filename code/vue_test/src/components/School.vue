<template>
    <div>
        <h2>学校名称：{{schoolName}}</h2>
        <h2>学校地址：{{address}}</h2>
        <button @click="showName">点我提示学校名</button>
    </div>
</template>

<script>
export default {
    name: 'School',
    methods: {
        showName(){
            alert(this.schoolName)
        }
    },

    // props:['schoolName', 'address']
    props:{
        schoolName:{
            type:String,
            required:true //必要的
        },
        address:{
            type:String,
            default:"beijing" //默认值
        }
    }
};
</script>
