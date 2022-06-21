<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <p>debug sort:{{currentSort}} direction: {{ currentSortDir}}</p>
        <!-- table -->
        <table>

          <!-- head -->
          <thead>
            <tr>
              <th>Image</th>
              <th @click="sort('firstName')">Name</th>
              <th @click="sort('age')">Age</th>
              <th @click="sort('gender')">Gender</th>
            </tr>
          </thead>

          <!-- body -->
          <tbody>
            <tr v-for="user in usersSort" :key="user.id">
              <td><img :src="user.image" alt="" width="100"></td>
              <td>{{ user.firstName }}</td>
              <td>{{ user.age }}</td>
              <td>{{ user.gender }}</td>
            </tr>
          </tbody>
        </table>        

      </div>
    </section>
  </div>
</template>

<script>
  import axios from "axios"
  export default {
    data() {
      return {
        users: [

        ],
        currentSort: '',
        currentSortDir: 'asc'

      }
    },
    created() {
      axios
        .get('https://dummyjson.com/users/')
        .then(response => {
          this.users = response.data.users
        })
        .catch(error => console.error(error))

      // this.users = [
      //   { id: 1, name: 'Jack', age: 22, gender: 'male' },
      //   { id: 2, name: 'Alex', age: 24, gender: 'male' },
      // ]

    },
    computed: {
      usersSort() {
        return this.users.sort((a, b) => {
          let mod = 1
          if (this.currentSortDir === 'desc') mod = -1
          if (a[this.currentSort] < b[this.currentSort]) return -1 * mod
          if (a[this.currentSort] > b[this.currentSort]) return 1 * mod
          return 0
        })
      }
    },
    methods: {
      sort(e) {
        if (e === this.currentSort) {
          this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc'
        }
        this.currentSort = e
      }
    }
  }
</script>