<template>
  <div id="dashboard">
    <ul class="collection with-header">

      <li class="collection-header"><h4>Students</h4></li>
    </ul>
    <br>
    <div class="row">
      <div class="input-field">
        <input type="text" v-model="search" placeholder="search here" />
      </div>
    </div>

    <br>
    <ul class="collection with-header">
      <li v-for="student in filtered_search" v-bind:key="student.id" class="collection-item">
        <div class="chip">{{student.major}}</div>
        {{student.student_id}}:{{student.student_name}}
        <router-link class="secondary-content" v-bind:to="{name:
        'view-student', params: {student_id: student.student_id}}">
          <i class="fa fa-eye">View</i>
        </router-link>
      </li>
    </ul>

    <!-- <div class="fixed-action-btn">
       <router-link to="/new" class="btn-floating btn-large red"> -->
    <!-- <i class="fa fa-plus"></i> -->
    <div>
      <router-link to="/new" class="btn red">New Student
      </router-link>
    </div>
  </div>
</template>

<script>
import db from './firebaseinit'
export default {
  name: 'dashboard',
  data(){
    return{
      student: [],
      search: ""
    }
  },
  created (){
    //runs after component is created/loaded automatically
    db.collection('student').orderBy('student_id').get().then(querySnapshot =>
    {
      querySnapshot.forEach(doc => {
        console.log(doc.data())
        const data = {
          'id': doc.id,
          'student_id': doc.data().student_id,
          'student_name': doc.data().student_name,
          'major': doc.data().major,
          'degree_type' : doc.data().degree_type,
          'year_of_graduation': doc.data().year_of_graduation
        }
        this.student.push(data)
      })
    })
  },
  computed:{    filtered_search: function(){
      return this.student.filter((student) =>{
        return student.student_name.toLowerCase().match(this.search.toLowerCase()) ||
          student.major.toLowerCase().match(this.search.toLowerCase()) ||
          student.student_id.match(this.search)
      })
    }
  }

}
</script>
