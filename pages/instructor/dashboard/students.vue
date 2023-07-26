<template>
  <div>
    <div class="flex justify-between bg-green-900 px-10 py-3">
      <p class="text-white font-semibold text-lg">Students</p>
      <modal />
    </div>
    <div class="flex justify-between mx-5 mt-10">
      <div class="mb-4">
        <label for="emailFilter" class="block font-medium mb-1"
          >Filter by email:</label
        >
        <input
          v-model="emailFilter" id="emailFilter" type="text" class="border border-gray-300 rounded-md px-4 py-1 w-full" placeholder="Search by email"
        />
      </div>
      <div>
        <!-- Add Button -->
        <div class="bg-green-900 mt-10 px-2 py-1 rounded-lg cursor-pointer" @click="this.openAddModal">
          <p class="font-semibold text-sm text-white">Add new</p>
        </div>

        <!-- Modal -->
        <div v-if="addModal" class="z-10 pt-24 absolute inset-0 flex items-center justify-center" >
          <div class="bg-white rounded-lg shadow-md p-5 overflow-y-auto">
            <!-- Modal Content -->
            <div class="flex justify-between">
              <h2 class="text-2xl font-bold mb-4">{{ addModalHeading }}</h2>
              <button class="bg-red-500 text-white text-xl font-xl px-3 py-1 mt-4 rounded-md" @click="closeAddModal()" > X </button>
            </div>
            <p v-if="loading">adding new user</p>
            <form v-else @submit.prevent="addStudent()" class="grid grid-cols-2 gap-12 bg-white shadow-md rounded px-8 py-6 mb-4" >
            
              <!-- First Name -->
              <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="first_name" >First name:</label >
                <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="first_name" type="text" v-model="student.first_name"/>
                <p v-if="this.errors.first_name" class="text-sm text-red-600 text-left mb-2" >*{{ this.errors.first_name }}</p>
              </div>
              <!-- Last Name -->
              <div class="mb-4">
                <label for="last_name">Last Name:</label>
                <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="last_name" type="text" v-model="student.last_name" />
                <p v-if="this.errors.last_name" class="text-sm text-red-600 text-left mb-2" > *{{ this.errors.last_name }}</p>
              </div>
              <!--National ID -->
              <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="national_id" >National ID Number:</label >
                <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="national_id" type="text" v-model="student.national_id"/>
                <p v-if="this.errors.national_id" class="text-sm text-red-600 text-left mb-2" > *{{ this.errors.national_id }} </p>
              </div>
              <!-- Class Type -->
              <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="email" >Class Type:</label >
                <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="class_type" type="class_type" v-model="student.class_type" />
                <p v-if="this.errors.class_type" class="text-sm text-red-600 text-left mb-2" > *{{ this.errors.class_type }} </p>
              </div>
              
              <!-- Submit Button -->
              <div class="ml-2">
                <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" > Submit </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div class="relative overflow-x-auto shadow-md sm:rounded-lg mx-5">
      <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
        <thead
          class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
        >
          <tr>
            <th scope="col" class="px-6 py-3">Date</th>
            <th scope="col" class="px-6 py-3">First Name</th>
            <th scope="col" class="px-6 py-3">Last Name</th>
            <th scope="col" class="px-6 py-3">National ID</th>
            <th scope="col" class="px-6 py-3">Class Type</th>
            <!-- <th scope="col" class="px-6 py-3"></th>
            <th scope="col" class="px-6 py-3"></th> -->
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="item in filteredItems"
            :key="item._id"
            class="bg-white border-b dark:bg-gray-900 dark:border-gray-700"
          >
          <td class="px-6 py-4">{{ item.created_at }}</td>
            <td class="px-6 py-4">{{ item.first_name }}</td>
            <td class="px-6 py-4">{{ item.last_name }}</td>
            <td class="px-6 py-4">{{ item.national_id }}</td>
            <td class="px-6 py-4">{{ item.class_type }}</td>
            <!-- <td class="px-2 py-4">
              <a href="#" class="font-medium text-green-500 dark:text-blue-500 hover:underline">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" @click="fetchStudentByID(item.national_id)" class="bi bi-pencil-square" viewBox="0 0 16 16"> <path d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z"/> <path fill-rule="evenodd" d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z"/> </svg>
              </a >
            </td>
            <td class="px-2 py-4">
              <a href="#" class="font-medium text-yellow-500 dark:text-blue-500 hover:underline" >
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16"> <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/> <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/> </svg>
              </a >
            </td> -->
          </tr>
        </tbody>
      </table>
    </div>
    <div v-if="editModal" class="z-10 ml-40 pt-72 absolute inset-0 flex items-center justify-center">
      <div class="bg-white rounded-lg shadow-md p-5 overflow-y-auto">
        <!-- Modal Content -->
        <div class="flex justify-between">
          <h2 class="text-2xl font-bold mb-4">{{ editModalHeading }}</h2>
          <button class="bg-red-500 text-white text-xl font-xl px-3 py-1 mt-4 rounded-md" @click="closeEditModal">X</button>
        </div>
        <p v-if="loading">Edit Student</p>
            <form v-else @submit.prevent="editStudent()" class="grid grid-cols-2 gap-12 bg-white shadow-md rounded px-8 py-6 mb-4" >
              <!-- First Name -->
              <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="first_name" >First name:</label >
                <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="first_name" type="text" v-model="student.first_name"/>
                <p v-if="this.errors.firstname" class="text-sm text-red-600 text-left mb-2" >*{{ this.errors.firstname }}</p>
              </div>
              <!-- Last Name -->
              <div class="mb-4">
                <label for="last_name">Last Name:</label>
                <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="last_name" type="text" v-model="student.last_name" />
                <p v-if="this.errors.last_name" class="text-sm text-red-600 text-left mb-2" > *{{ this.errors.last_name }}</p>
              </div>
              <!-- Class Type -->
              <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="email" >Class Type:</label >
                <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="class_type" type="class_type" v-model="student.class_type" />
                <p v-if="this.errors.class_type" class="text-sm text-red-600 text-left mb-2" > *{{ this.errors.class_type }} </p>
              </div>
              <!-- Password -->
              <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="national_id" >National Id:</label >
                <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="national_id" type="text" v-model="student.national_id"/>
                <p v-if="this.errors.national_id" class="text-sm text-red-600 text-left mb-2" > *{{ this.errors.national_id }} </p>
              </div>
              <!-- Submit Button -->
              <div class="ml-2">
                <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" > Submit </button>
              </div>
            </form>       
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      addModal: false,
      addModalHeading: "Add new user",
      idNumberFilter: "",
      emailFilter: "",
      loading: false,
      editModal: false,
      deleteModal: false,
      editModalHeading: 'Edit Student',
      errors: {},
      student: {
        last_name: '',
        national_id: '',
        email: '',
        first_name: '',
      },
      item: [
      
      ],
    };
  },
  computed: {
    filteredItems() {
      let filteredItems = this.item;

      if (this.emailFilter) {
        filteredItems = filteredItems.filter((items) =>
          items.email.toLowerCase().includes(this.emailFilter.toLowerCase())
        );
      }

      return filteredItems;
    },
  },
  methods: {
    openAddModal() {
      this.addModal = true;
    },
    closeAddModal() {
      this.addModal = false;
    },
    async addStudent() {
      this.errors = {};
      if (!this.student.first_name) {
        this.errors.first_name = "First Name is required";
      }
      if (!this.student.last_name) {
        this.errors.last_name = "Last Name is required";
      }
      if (!this.student.email) {
        this.errors.email = "Email is required";
      }
      if (!this.student.class_type) {
        this.errors.class_type = "Class type is required";
      }
      if (!this.student.national_id) {
        this.errors.national_id = "National ID is required";
      }
      if (Object.keys(this.errors).length === 0) {
        // make API call or submit form data here
        try {
          await axios.post("http://localhost:8000/students",
              {
                last_name: this.student.last_name,
                email: this.student.email,
                first_name: this.student.first_name,
                class_type:  this.student.class_type,
                national_id: this.student.national_id

              },
              {
                headers: { "Content-Type": "application/json",
                Authorization : 'Bearer ' + localStorage.token,
              // 'Access-Control-Allow-Origin':'*'
             },
                // credentials: "include"
              }
            )
            .then((response) => {
              const data = response.data;
              alert("Student added successfully.");
              this.response = data;
              console.log(response);
            });
        } catch (err) {
          console.log("Error:", err);
          this.errors.failed = "Sorry, an error occured!";
          this.errors.ERR = err;
        }
        console.log("Form submitted successfully");
      }
    },
    async getAllStudent(){
    this.loading = true;
    const URL = "http://localhost:8000/getStudents";
    const token = localStorage.token;
    await axios.get(URL, {
                headers: { "Content-Type": "application/json",
                Authorization : 'Bearer ' + localStorage.token,
              // 'Access-Control-Allow-Origin':'*'
             },
    }).then((res) =>
     {
      this.item = res.data;
      console.log("Fetching Data Completed...");
    }) .catch(error => {
      console.log(error.code)
      this.error=error.code;
      this.errored = true

    }).finally(() => this.loading = false);
    },
    async fetchStudentByID(ID){
          this.loading = true;
    const URL = `http://localhost:8000/students/${ID}`;
    // const token = localStorage.token;
    await axios.get(URL,{
      headers: { "Content-Type": "application/json",
                Authorization : 'Bearer ' + localStorage.token,
              // 'Access-Control-Allow-Origin':'*'
             },
    }).then((res) =>
     {
      this.student = res.data
      console.log(this.student);
      console.log("Information tatora baba.");
      this.editModal = true;
    }) .catch(error => {
      console.log(error.code)
      this.error=error.code;
      this.errored = true

    }).finally(() => this.loading = false);
    },
    async updateStudent() {
      this.errors = {};
      if (!this.student.first_name) {
        this.errors.first_name = "Name is required";
      }
      if (!this.student.email) {
        this.errors.email = "Email is required";
      }
      if (!this.student.address) {
        this.errors.address = "Address is required";
      }
      if (!this.student.phone) {
        this.errors.phone = "Phone number is required";
      }
      if (Object.keys(this.errors).length === 0) {
        // make API call or submit form data here
        try {
          await axios.put("http://localhost:8000/update/organizations",
              {
                first_name: this.student.first_name,
                last_name: this.student.last_name,
                class_type: this.student.class_type

              },
              {
                headers: { "Content-Type": "application/json",
                Authorization : 'Bearer ' + localStorage.token,
              // 'Access-Control-Allow-Origin':'*'
             },
                // credentials: "include"
              }
            )
            .then((response) => {
              const data = response.data;
              alert("Organization updated successfully.");
              this.response = data;
              console.log(response);
            });
        } catch (err) {
          console.log("Error:", err);
          this.errors.failed = "Sorry, an error occured!";
          this.errors.ERR = err;
        }
        console.log("Form submitted successfully");
      }
    },
    openEditModal() {
        this.editModal = true;
    },
    closeEditModal() {
      this.editModal = false;
    },
    openDeleteModal(id) {
      this.deleteModal = true;
      this.FID = id;
      },
      closeDeleteModal() {
        this.deleteModal = false;
      },
  },
  mounted(){
    this.getAllStudent()
  }
};
</script>

<style>
</style>