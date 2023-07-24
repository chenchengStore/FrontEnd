<template>
  <div class="demo">
    <School/>
    <Student/>
  </div>
</template>

<script>
import School from './School.vue'
import Student from './Student.vue'
export default {
    // '<v'可以快捷输入模板内容
    name:'App',
    components:{
        School,
        Student
    }
}
</script>

<style>
    .demo{
        background-color: red;
    }
</style>