<template>
  <div class="container mx-auto p-8">
    <h1 class="text-3xl font-bold mb-4">Users</h1>

    <!-- Form to Add Users -->
    <div class="mb-8">
      <h2 class="text-xl font-semibold mb-2">Add User</h2>
      <form @submit.prevent="createUser" class="flex flex-col space-y-4 max-w-xs">
        <input type="text" v-model="newUser.name" placeholder="Name" required class="p-2 border rounded border-gray-300 focus:outline-none focus:border-blue-500">
        <input type="email" v-model="newUser.email" placeholder="Email" required class="p-2  border rounded border-gray-300 focus:outline-none focus:border-blue-500">
        <button type="submit" class="bg-green-500 hover:bg-green-600 text-white font-semibold px-4 py-2 rounded">
          Add User
        </button>
      </form>
    </div>

    <!-- Display Users -->
    <h2 class="text-xl font-semibold mb-4">User List</h2>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
      <div v-for="user in users" :key="user.id" class="bg-white rounded-lg shadow-md p-4">
        <h2 class="text-xl font-semibold mb-2">{{ user.name }}</h2>
        <p class="text-gray-600 mb-4">{{ user.email }}</p>
        <div class="flex justify-end">
          <button @click="deleteUser(user.id)" class="bg-red-500 hover:bg-red-600 text-white font-semibold px-4 py-2 rounded mr-2">
            Delete
          </button>
          <button @click="editUser(user)" class="bg-blue-500 hover:bg-blue-600 text-white font-semibold px-4 py-2 rounded">
            Edit
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      users: [],
      newUser: {
        name: '',
        email: ''
      }
    };
  },
  mounted() {
    this.fetchUsers();
  },
  methods: {
    fetchUsers() {
      axios.get('https://jsonplaceholder.typicode.com/users')
        .then(response => {
          this.users = response.data;
        })
        .catch(error => {
          console.error('Error fetching users:', error);
        });
    },
    createUser() {
      axios.post('https://jsonplaceholder.typicode.com/users', this.newUser)
        .then(response => {
          this.users.push(response.data);
          this.newUser = { name: '', email: '' };
        })
        .catch(error => {
          console.error('Error creating user:', error);
        });
    },
    deleteUser(userId) {
      axios.delete(`https://jsonplaceholder.typicode.com/users/${userId}`)
        .then(() => {
          this.users = this.users.filter(user => user.id !== userId);
        })
        .catch(error => {
          console.error('Error deleting user:', error);
        });
    },
    editUser() {
      // Implement your logic to edit/update a user
      // This could involve displaying a form for editing and sending a PUT/PATCH request to update the user details
    }
  }
};
</script>

<style>
/* Add your component styles here if needed */
</style>
