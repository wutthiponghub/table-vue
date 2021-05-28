<template>
  <p v-if="loading">
    Still loading..
  </p>
  <table class="table table-striped table-hover">
    <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">Name</th>
        <th scope="col">Username</th>
        <th scope="col">Avatar</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="person in people" :key="person.id">
        <th scope="row">{{ person.id }}</th>
        <td>{{ person.fname }} {{ person.lname }}</td>
        <td>{{ person.username }}</td>
        <td>
          <img v-bind:src="person.avatar" style="width: 30px" />
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  data() {
    return {
      people: [],
      loading: false,
    };
  },
  mounted() {
    this.loading = true;
    fetch("https://www.mecallapi.com/api/users")
      .then((res) => {
        return res.json();
      })
      .then((person) => {
        this.people.push(...person);
        this.loading = false;
        console.log(this.people);
      })
      .catch((error) => {
        console.log(error);
      });
  },
};
</script>
