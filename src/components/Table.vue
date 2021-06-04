<template>
  <p v-if="loading">
    Still loading..
  </p>
  <table v-else class="table table-striped table-hover">
    <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">Name</th>
        <th scope="col">Username</th>
        <th scope="col">Avatar</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="person in people"
        :key="person.id"
        @click="readOne(person.id)"
        style="cursor: pointer"
        data-bs-toggle="modal"
        data-bs-target="#ModalForm"
      >
        <th scope="row">{{ person.id }}</th>
        <td>{{ person.fname }} {{ person.lname }}</td>
        <td>{{ person.username }}</td>
        <td>
          <img v-bind:src="person.avatar" style="width: 30px" />
        </td>
      </tr>
    </tbody>
  </table>

  <div class="text-center">
    <button
      type="button"
      class="btn btn-primary"
      data-bs-toggle="modal"
      data-bs-target="#ModalForm"
      @click="
        clearForm();
        modalView = false;
      "
    >
      Show Form
    </button>
  </div>

  <!-- Modal -->
  <div
    class="modal fade"
    id="ModalForm"
    tabindex="-1"
    aria-labelledby="ModalFormLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 v-if="modalView" class="modal-title" id="ModalFormLabel">
            View
          </h5>
          <h5 v-else class="modal-title" id="ModalFormLabel">
            Create
          </h5>
          <button
            id="closeButton"
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body text-center">
          <div v-if="modalView">
            <img v-bind:src="person.avatar" style="width: 30px" />
          </div>
          <div v-else></div>
          <form v-on:submit.prevent @submit="create()">
            <div class="form-group">
              <label for="fname">First name</label>
              <input
                type="text"
                class="form-control"
                id="fname"
                name="fname"
                v-model="person.fname"
                required
              />
            </div>
            <div class="form-group">
              <label for="lname">Last name</label>
              <input
                type="text"
                class="form-control"
                id="lname"
                name="lname"
                v-model="person.lname"
                required
              />
            </div>
            <div class="form-group">
              <label for="username">Username</label>
              <input
                type="text"
                class="form-control"
                id="username"
                name="username"
                v-model="person.username"
                required
              />
            </div>
            <div class="form-group">
              <label for="email">Email</label>
              <input
                type="email"
                class="form-control"
                id="email"
                name="email"
                v-model="person.email"
                required
              />
            </div>
            <br />

            <p v-if="modalView"></p>
            <button v-else type="submit" class="btn btn-primary">
              Create
            </button>
          </form>
        </div>
        <div v-if="modalView" class="modal-footer">
          <button type="button" class="btn btn-primary" @click="update()">
            Update
          </button>
          <button type="button" class="btn btn-primary" @click="deletef()">
            Delete
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {
  getPeople,
  createPerson,
  getPerson,
  updatePerson,
  deletePerson,
} from "../services/People";

export default {
  data() {
    return {
      people: [],
      person: {},
      loading: false,
      modalView: false,
    };
  },
  methods: {
    clearForm() {
      this.person = {};
    },

    readAll() {
      getPeople()
        .then((result) => {
          this.people = result;
          this.loading = false;
          console.log(this.people);
          document.getElementById("closeButton").click();
        })
        .catch((error) => {
          console.log(error);
        });
    },

    create() {
      this.person.avatar = "https://www.mecallapi.com/users/cat.png";
      const data = this.person;
      createPerson(data).then(
        (result) => {
          this.readAll();
          console.log(result);
        },
        (error) => {
          console.log(error);
        }
      );
    },

    readOne(id) {
      this.modalView = true;
      getPerson(id)
        .then((result) => {
          this.person = result.user;
          console.log(this.person);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    update() {
      const data = this.person;
      updatePerson(data)
        .then((result) => {
          this.readAll();
          console.log(result);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    deletef() {
      const data = this.person.id;
      deletePerson(data)
        .then((result) => {
          this.readAll();
          console.log(result);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  mounted() {
    this.loading = true;
    this.readAll();
  },
};
</script>
