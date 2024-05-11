
<template>
  <div class="container-fluid mt-3">
    <div class="card">

      <div class="card-header">
        <h4>
          Students
          <RouterLink to="/students/create" class="btn btn-primary float-end">
            Add Student
          </RouterLink>
        </h4>
      </div>

      <div class="card-body">
        <table class="table table-bordered">
          <thead>
            <tr>
              <th>ID</th>
              <th>Name</th>
              <th>Course</th>
              <th>Email</th>
              <th>Phone</th>
              <th>Created At</th>
              <th>Action</th>
            </tr>
          </thead>

          <tbody v-if="this.students.length > 0">
            <tr v-for="(student, index) in this.students" :key="index">
              <td>{{ student.id }}</td>
              <td>{{ student.name }}</td>
              <td>{{ student.course }}</td>
              <td>{{ student.email }}</td>
              <td>{{ student.phone }}</td>
              <td>{{ student.created_at }}</td>
              <td>
                <RouterLink :to="{ path: `/students/${student.id}/edit` }" class="btn btn-success">
                  Edit
                </RouterLink>
                <button type="button" @click="deleteStudent(student.id)" class="btn btn-danger mx-2">
                  Delete
                </button>
              </td>
            </tr>
          </tbody>

          <tbody v-else>
              <tr>
                <td colspan="7">
                  <div class="d-flex justify-content-center">
                    <div class="loader"></div>
                  </div>
                </td>
              </tr>
          </tbody>
        </table>
      </div>

    </div>
  </div>
</template>

<style scoped>
  /* HTML: <div class="loader"></div> */
  .loader {
    width: 50px;
    padding: 8px;
    aspect-ratio: 1;
    border-radius: 50%;
    background: #25b09b;
    --_m: 
      conic-gradient(#0000 10%,#000),
      linear-gradient(#000 0 0) content-box;
    -webkit-mask: var(--_m);
            mask: var(--_m);
    -webkit-mask-composite: source-out;
            mask-composite: subtract;
    animation: l3 1s infinite linear;
  }
  @keyframes l3 {to{transform: rotate(1turn)}}
</style>

<script>
import axios from 'axios';

export default {
  name: 'students',

  data() {
    return {
      students: []
    }
  },

  mounted() {
    this.getStudents();
  },

  methods: {
    getStudents() {
      axios.get('http://127.0.0.1:8000/api/students')
           .then(res => {
            this.students = res.data.students;
           });
    },
    async deleteStudent(studentId) {
      if(confirm('Are You Sure, You Want To Delete This Data?'))

        // ========CONTOH MENGGUNAKAN THEN DAN CATCH========
        // axios.delete(`http://127.0.0.1:8000/api/students/${studentId}/delete`)
        //     .then(res => {
        //       alert(res.data.message);
        //       this.getStudents();
        //     })
        //     .catch(error => {
        //       if(error.response) 
        //         if(error.response.status === 404)
        //           alert(error.response.data.message);
        //     });
        // ========CONTOH MENGGUNAKAN THEN DAN CATCH========



        // ========CONTOH MENGGUNAKAN ASYNC DAN AWAIT========
        try
        {
          const response = await axios.delete(`http://127.0.0.1:8000/api/students/${studentId}/delete`);
          alert(response.data.message);
          this.getStudents();
        }
        catch(error)
        {
          if(error.response) 
            if(error.response.status === 404)
              alert(error.response.data.message);
        }
        // ========CONTOH MENGGUNAKAN ASYNC DAN AWAIT========

    }
  }
}

</script>
