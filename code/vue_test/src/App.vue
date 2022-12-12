<template>
  <div>
      <Student/>
      <school schoolName="北邮" address="北京"/>
  </div>
</template>

<script>
import School from './components/School.vue';
import Student from './components/Student.vue';
export default {
  name: 'APP',
  components:{
      Student,
      School
  }
};
</script>