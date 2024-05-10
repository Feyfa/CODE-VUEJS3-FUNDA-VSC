<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">
        <h4>Add Student</h4>
      </div>

      <div class="card-body">
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
          <button type="button" @click="saveStudent" class="btn btn-primary">Save</button>
        </div>
      </div>
    </div>
  </div>
</template>




<script>
import axios from 'axios';


export default {
  name: 'studentCreate',
  
  data() {
    return {
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
  
  methods: {
    saveStudent() {
      axios.post('http://127.0.0.1:8000/api/students',
        this.model.student
      )
      .then(res => {

        console.log(res);
        alert(res.data.message);
      
        this.model.student = {
          name: '',
          course: '',
          email: '',
          phone: '',
        }
        
      })
      .catch(error => {
        if(error.response) 
        {
          if(error.response.status === 422)
          {
            this.errorList = error.response.data;
          }

          console.log(error.response.data);
          console.log(error.response.status);
          console.log(error.response.headers);
        }
        else if(error.request)
        {
          console.log(error.request);
        }
        else
        {
          console.log('Error', error.message);
        }

      });

    }
  }
}

</script>