<template>
  <div>
    <div class="container-fluid bg-dark text-light text-center">
      <h1>students data list</h1>
    </div>
    <div class="container mt-4">
      <div class="row p-3" style="justify-content: space-between">
        <h2>register user</h2>
        <button class="btn bg-info text-light" @click="addmodal = true">
          add user
          <img class="icon" src="../assets/User_perspective_matte.png" />
        </button>
      </div>
      <hr class="bg-info" style="height: 1px" />
    </div>
    <div class="container">
      <table class="table-bordered table striped">
        <thead>
          <tr class="text-center bg-info text-light">
            <td>ID</td>
            <td>Name</td>
            <td>score</td>
            <td>Phone</td>
            <td>Delete</td>
          </tr>
        </thead>
        <tbody id="data" class="text-center">
          <tr v-bind:key="items.id" v-for="items in items">
            <td>{{ items.id }}</td>
            <td>{{ items.name }}</td>
            <td>{{ items.score }}</td>
            <td>{{ items.phone }}</td>
            <td>
              <!--  Button deletemodal -->
              <button
                type="button"
                class="btn"
                @click="deleteButtom(items.id, items.name)"
              >
                <img class="icon" src="../assets/Error_perspective_matte.png" />
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!--  modals -->
    <div id="overly"></div>
    <!-- addtemodal -->
    <div v-if="addmodal">
      <div class="overly">
        <div class="modalM">
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <div class="row pl-3">
                  <img
                    class="icon"
                    src="../assets/User_perspective_matte.png"
                  />
                  <h5 class="modal-title" id="exampleModalLabel">
                    &nbsp; Add User
                  </h5>
                </div>
                <button
                  type="button"
                  class="btn"
                  @click="addmodal = false"
                  data-bs-dismiss="modal"
                  aria-label="Close"
                >
                  &times;
                </button>
              </div>
              <div class="modal-body">
                <form action="#" method="post">
                  <div class="form-group pt-3">
                    <input
                      type="text"
                      name="Name"
                      class="form-control form-control-lg"
                      placeholder="ID"
                      v-model="id"
                    />
                  </div>
                  <div class="form-group pt-3">
                    <input
                      type="text"
                      name="Name"
                      class="form-control form-control-lg"
                      placeholder="Name"
                      v-model="name"
                    />
                  </div>
                  <div class="form-group pt-3">
                    <input
                      type="number"
                      name="score"
                      class="form-control form-control-lg"
                      placeholder="score"
                      v-model="score"
                    />
                  </div>
                  <div class="form-group pt-3">
                    <input
                      type="tel"
                      name="phone"
                      class="form-control form-control-lg"
                      placeholder="Phone"
                      v-model="phone"
                    />
                  </div>
                </form>
              </div>
              <div class="modal-footer">
                <button
                  type="button"
                  @click="addmodal = false"
                  class="btn btn-secondary"
                  data-bs-dismiss="modal"
                >
                  Cancel
                </button>
                <button type="button" class="btn btn-primary" @click="addData">
                  Add User
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- deletemodal -->
    <div v-if="deletemodal">
      <div class="overly">
        <div class="modalM">
          <div class="modal-dialog" id="delete">
            <div class="modal-content">
              <div class="modal-header">
                <div class="row pl-3">
                  <img class="icon" src="../assets/emergency.png" />
                  <h5 class="modal-title" id="exampleModalLabel">
                    &nbsp; Delete User
                  </h5>
                </div>
                <button
                  type="button"
                  class="btn"
                  @click="deletemodal = false"
                  data-bs-dismiss="modal"
                  aria-label="Close"
                >
                  &times;
                </button>
              </div>
              <div class="modal-body">
                <p>{{ deleteMessage }}</p>
              </div>
              <div class="modal-footer">
                <button
                  type="button"
                  class="btn btn-danger"
                  @click="deletemodal = false"
                  data-bs-dismiss="modal"
                >
                  No
                </button>
                <button type="button" class="btn btn-primary" @click="Delete">
                  Yes
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "grid",
  data: function () {
    return {
      deletemodal: false,
      addmodal: false,
      editmodal: false,
      items: [],
      deleteMessage: "",
      tempId: "",

      id: "",
      name: "",
      score: "",
      phone: "",
    };
  },
  mounted() {
    fetch("http://localhost:3000/items")
      .then((res) => res.json())
      .then((data) => (this.items = data))
      .catch((err) => console.log(err.message));
  },

  methods: {
    addData() {
      
     this.items.push({
        id: this.id,
        name: this.name,
        score: this.score,
        phone: this.phone,
      });

      

      this.addmodal = false;
      console.log(this.items);

      this.id = "";
      this.name = "";
      this.score = "";
      this.phone = "";
    },
    deleteButtom(id, name) {
      this.deleteMessage =
        "are you sure you want to delete the user " + name + " ?";
      this.deletemodal = true;
      this.tempId = this.findIndex(id);
      console.log(this.tempId);
    },
    Delete() {
      this.deletemodal = false;
      this.items.splice(this.tempId, 1);
      this.tempId = "";
    },
    findIndex(id) {
      for (let i = 0; i < this.items.length; i++) {
        if (this.items[i].id == id) return i;
      }
      return -1;
    },
  },
};
</script>
<style lang="css">
.overly {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  background-color: rgb(0, 0, 0, 0.3);
}
.icon {
  width: 26px;
  height: 26px;
}
.modalM {
  top: 55px;
  width: 100%;
  position: fixed;
  z-index: 2;
}
</style>