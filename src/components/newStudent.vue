<template>
  <div id="new-employee">
    <h3>New Student</h3>
    <div class = "row">
      <form @submit.prevent="saveStudent" class="col s12">
        <!-- ID -->
        <div class="row">
          <div class="input-field col s12">
            <input type="number" v-model="student_id" PLACEHOLDER="Student ID Ex. 001, 002" required>
          </div>
        </div>
        <!-- Name -->
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="student_name" PLACEHOLDER="Student Name Ex. John Hopkins" required>
          </div>
        </div>
        <!-- Department -->
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="degree" placeholder="Degree Ex. BS, MS" required>

          </div>
        </div>
        <!-- Entry for Position-->
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="major" placeholder="Major Ex. Computer Science, Mechanical" required>

          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="number" v-model="graduation_year" placeholder="Graduation Year Ex. 2021" required>

          </div>
        </div>
        <button type="submit" class="btn">Submit</button>
        <router-link to="/" class="btn grey">Cancel</router-link>
      </form>
    </div>
  </div>
</template>

<script>
import db from './firebaseinit'
export default {
  name: 'new-student',
  data(){
    return{
      student_id: null,
      student_name: null,
      degree: null,
      major: null,
      graduation_year: null
    }
  },
  methods:{
    saveStudent(){
      db.collection('student').add({
        student_id: this.student_id,
        student_name: this.student_name,
        degree_type: this.degree,
        major: this.major,
        year_of_graduation: this.graduation_year
        })
        .then(docRef => {
          this.$router.push('/')
        }).catch(error => console.log('Something wrong happened'))
    }
  }
}
</script>
