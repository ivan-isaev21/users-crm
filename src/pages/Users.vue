<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">

        <!-- table -->
        <table>

          <!-- head -->
          <thead>
            <tr>
              <th>Name</th>
              <th>Age</th>
              <th>Gender</th>
            </tr>
          </thead>

          <!-- body -->
          <tbody>
            <tr>
              <td></td>
            </tr>
          </tbody>
        </table>

      </div>
    </section>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        users: [
          { id: 1, name: 'Jack', age: 22, gender: 'male' },
          { id: 2, name: 'Alex', age: 24, gender: 'male' },
        ]
      }
    }
  }
</script>