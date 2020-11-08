<template>
  <div id="edit-employee">
    <h3>Edit Employee</h3>
    <div class = "row">
      <form @submit.prevent="updateStudent" class="col s12">


        <!-- ID -->
        <div class="row">
          <div class="input-field col s12">
            Student ID: <input disabled type="text" v-model="student_id" required>
          </div>
        </div>


        <!-- Name -->
        <div class="row">
          <div class="input-field col s12">
            Student Name: <input type="text" v-model="student_name" required>
          </div>
        </div>


        <!-- Department -->
        <div class="row">
          <div class="input-field col s12">
            Major: <input type="text" v-model="major" required>
          </div>
        </div>


        <!-- Entry for Position-->
        <div class="row">
          <div class="input-field col s12">
            Degree: <input type="text" v-model="degree" required>
          </div>
        </div>

        <div class="row">
          <div class="input-field col s12">
           Graduation Year <input type="text" v-model="year_of_graduation" required>
          </div>
        </div>

        <button type="submit" class="btn">Confirm</button>
        <router-link to="/" class="btn grey">Cancel</router-link>
      </form>
    </div>
  </div>
</template>

<script>
import db from './firebaseinit'
export default {
  name: 'edit-student',
  data(){
    return{
      student_id: null,
      student_name: null,
      major: null,
      degree: null,
      year_of_graduation: null
    }
  },
  beforeRouteEnter (to, from, next){
    db.collection('student').where('student_id',
      '==', to.params.student_id).get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          next(vm => {
            vm.student_id = doc.data().student_id
            vm.student_name = doc.data().student_name
            vm.major = doc.data().major
            vm.degree = doc.data().degree_type
            vm.year_of_graduation = doc.data().year_of_graduation
          })
        })
      })
  },
  methods: {

    updateStudent() {
      db.collection('student').where
      ('student_id', '==',
        this.$route.params.student_id).get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            doc.ref.update({
              student_id: this.student_id,
              student_name: this.student_name,
              major: this.major,
              degree: this.degree,
              year_of_graduation: this.year_of_graduation
            }).then(() => {
              this.$router.push({name: 'view-student',
              params: {student_id: this.student_id}})
            })
          })
        })
    }
  }
}
</script>
