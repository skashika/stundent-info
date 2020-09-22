<template>
  <div id="dashboard">
    <ul class="collection with-header">
      <li class="collection-header"><h4>Employees</h4></li>
      <input type="text" v-model="search" placeholder="search here" />
      <!--<li v-for="employee in filtered_search" v-bind:key="employee.id" class="collection-item">-->
      <li v-for="employee in filtered_search" v-bind:key="employee.id" class="collection-item">
        <div class="chip">{{employee.dept}}</div>
        {{employee.employee_id}}:{{employee.name}}

        <router-link class="secondary-content" v-bind:to="{name:
        'view-employee', params: {employee_id: employee.employee_id}}">
          <i class="fa fa-eye"></i>
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
      employee: [],
      search: ""
    }
  },
  created (){
    //runs after component is created/loaded automatically
    db.collection('employees').orderBy('employee_id').get().then(querySnapshot =>
    {
      querySnapshot.forEach(doc => {
        console.log(doc.data())
        const data = {
          'id': doc.id,
          'employee_id': doc.data().employee_id,
          'name': doc.data().name,
          'dept': doc.data().dept,
          'position' : doc.data().position
        }
        this.employee.push(data)
      })
    })
  },
  computed:{
    filtered_search: function(){
      return this.employee.filter((employee) =>{
        return employee.name.match(this.search)
      })
    }
  }
}
</script>
