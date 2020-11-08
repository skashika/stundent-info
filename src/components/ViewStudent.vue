<template>
  <div id="view-student">
    <ul class="collection with-header">
      <li class="collection-header"><h4>Student ID: {{student_id}}</h4></li>
      <li class="collection-item">Student Name: {{name}}</li>
      <li class="collection-item">Major: {{major}}</li>
      <li class="collection-item">Degree: {{degree}}</li>
      <li class="collection-item">Year of Graduation: {{graduation_year}}</li>
    </ul>
    <router-link to="/" class="btn grey">Back</router-link>
    <router-link v-bind:to="{name: 'edit-student',
    params: {student_id: student_id}}" class="btn green">Edit</router-link>
    <button @click="deleteStudent" class="btn red">Delete</button>
  </div>
</template>

<script>
import db from './firebaseinit'
export default {
  name: 'view-student',
  data(){
    return{
      student_id: null,
      name: null,
      major: null,
      degree: null,
      graduation_year: null
    }
  },
  // execute before the route...before heading towards the next page #View Employee Page
  beforeRouteEnter (to, from, next){
    db.collection('student').where('student_id',
    '==', to.params.student_id).get()
    .then(querySnapshot => {
      querySnapshot.forEach(doc => {
        next(vm => {
          vm.student_id = doc.data().student_id
          vm.name = doc.data().student_name
          //console.log(vm.employee_id)
          vm.major = doc.data().major
          vm.dept = doc.data().dept
          vm.degree = doc.data().degree_type
          vm.graduation_year = doc.data().year_of_graduation
        })
      })
    })
  },
  methods: {

    deleteStudent (){
      if(confirm('Are you sure?')){
        db.collection('student').where
        ('student_id', '==',
          this.$route.params.student_id).get()
          .then(querySnapshot => {
            querySnapshot.forEach(doc => {
              doc.ref.delete()
              this.$router.push('/')
            })
          })
      }
    }
  }
}
</script>
