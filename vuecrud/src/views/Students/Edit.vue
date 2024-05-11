<template>
  <div class="container mt-3">
    <div class="card">
      <div class="card-header">
        <h4>Edit Student</h4>
      </div>

      <div class="card-body">
        <ul class="alert alert-warning" v-if="Object.keys(this.errorList).length > 0">
          <li class="mb-0 ms-3" v-for="(error, index) in this.errorList" :key="index">
            {{ error[0] }}
          </li>
        </ul>
        
        <div class="mb-3">
          <label for="">Name</label>
          <input v-model="model.student.name" type="text" class="form-control">
        </div>

        <div class="mb-3">
          <label for="">Course</label>
          <input v-model="model.student.course" type="text" class="form-control">
        </div>

        <div class="mb-3">
          <label for="">Email</label>
          <input v-model="model.student.email" type="text" class="form-control">
        </div>

        <div class="mb-3">
          <label for="">Phone</label>
          <input v-model="model.student.phone" type="text" class="form-control">
        </div>

        <div class="mb-3">
          <button type="button" @click="updateStudent" class="btn btn-primary">Update</button>
        </div>
      </div>
    </div>
  </div>
</template>




<script>
import axios from 'axios';

export default {
  name: 'studentEdit',
  
  data() {
    return {
      studentId: '',
      errorList: '',
      model: {
        student: {
          name: '',
          email: '',
          course: '',
          phone: '',
        }
      }
    }
  },  

  mounted() {
    this.studentId = this.$route.params.id;
    this.getStudenData(this.studentId);
  },
  
  methods: {
    getStudenData(studentId) {
      axios.get(`http://127.0.0.1:8000/api/students/${studentId}/edit`)
           .then(res => {
            this.model.student.name = res.data.student.name;
            this.model.student.email = res.data.student.email;
            this.model.student.course = res.data.student.course;
            this.model.student.phone = res.data.student.phone;
           })
           .catch(error => {
            if(error.response) 
              if(error.response.status === 404)
                alert(error.response.data.message);
           });
            
    },

    updateStudent() {

      axios.put(`http://127.0.0.1:8000/api/students/${this.studentId}/edit`,
        this.model.student
      )
      .then(res => {
        alert(res.data.message);

        this.errorList = '';

        window.location.href = '/students';
      })
      .catch(error => {      
        if(error.response) 
          if(error.response.status === 422)
            this.errorList = error.response.data.errors;
          else if(error.response.status === 404)
            alert(error.response.data.message);
        
        else if(error.request)
          console.log(error.request);
        
        else
          console.log('Error', error.message);
      });

    }
  }
}

</script>