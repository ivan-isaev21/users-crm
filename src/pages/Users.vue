<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">

        <!-- debug -->
        <p style="text-align:center;">
          <span>debug sort: {{ currentSort }}, direction: {{ currentSortDir}} </span>
          <span>page: {{ this.page.current }}, items per page: {{ this.page.length }} </span>
        </p>

        <!-- table -->
        <table>

          <!-- head -->
          <thead>
            <tr>
              <th></th>
              <th @click="sort('firstName')">Name {{sortIcon('firstName')}} </th>
              <th @click="sort('age')">Age {{sortIcon('age')}} </th>
              <th @click="sort('gender')">Gender {{sortIcon('gender')}} </th>
            </tr>
          </thead>

          <!-- body -->
          <tbody>
            <tr v-for="user in usersSort" :key="user.id">
              <td><img :src="user.image" alt=""></td>
              <td>{{ user.firstName }}</td>
              <td>{{ user.age }}</td>
              <td>{{ user.gender }}</td>
            </tr>
          </tbody>
        </table>

      </div>
    </section>

    <!-- buttons -->
    <section>
      <div class="container">
        <div class="button-list">
          <div @click="prevPage" class="btn btnPrimary">←</div>
          <div @click="nextPage" class="btn btnPrimary">→</div>
        </div>
      </div>
    </section>

  </div>
</template>

<script>
  import axios from "axios"
  export default {
    data() {
      return {
        users: [],
        currentSort: '',
        currentSortDir: 'asc',
        page: {
          current: 1,
          length: 3
        }
      }
    },
    created() {
      axios
        .get('https://dummyjson.com/users/')
        .then(response => {
          this.users = response.data.users
        })
        .catch(error => console.error(error))
    },
    computed: {
      usersSort() {
        return this.users.sort((a, b) => {
          let mod = 1
          if (this.currentSortDir === 'desc') mod = -1
          if (a[this.currentSort] < b[this.currentSort]) return -1 * mod
          if (a[this.currentSort] > b[this.currentSort]) return 1 * mod
          return 0
        }).filter((row, index) => {
          let start = (this.page.current - 1) * this.page.length
          let end = this.page.current * this.page.length
          if (index >= start && index < end) return true;
        })
      }
    },
    methods: {
      sort(e) {
        if (e === this.currentSort) {
          this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc'
        }
        this.currentSort = e
      },
      //pagination
      nextPage() {
        if ((this.page.current * this.page.length) < this.users.length) {
          this.page.current += 1
        }
      },
      prevPage() {
        if (this.page.current > 1) {
          this.page.current -= 1
        }
      },
      sortIcon(e) {
        if (e === this.currentSort && this.currentSortDir === 'desc') {
          return '↑'
        }
        return '↓'
      }
    }
  }
</script>
<style>
  img {
    width: 100px;
    height: auto;
    border-radius: 50%;
  }

  .button-list {
    width: 100%;
    text-align: center;
  }

  .btn {
    border-radius: 60px;
    margin: 0 20px;
  }
</style>