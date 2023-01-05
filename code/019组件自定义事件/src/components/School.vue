<template>
  <div class="demo">
    <h2>学校名称:{{  name }}</h2>
    <h2>学校地址: {{ address }}</h2>
    <button @click="sendSchoolName">把学校名传递给app</button>
  </div>
</template>

<script>
export default {
 name: "School",
 data(){
   console.log(this);
   return {
      name: '北邮',
      address: '北京'
   }
 },
 methods:{
   sendSchoolName(){
     this.getSchoolName(this.name);
   }
 },
 props: ['getSchoolName'],
}
</script>

<style scoped>
  /*scoped代表局部的*/
 .demo{
   background: skyblue;
   padding:5px
 }
</style>

