<template>
  <div class="hello">
    <button v-on:click="logoutUser">Logout</button>
    <div class="row">
      <div class="col-3"></div>
      <div class="col-2">Full name</div>
      <div class="col-2">date registration</div>
      <div class="col-2">temperature</div>
      <div class="col-3"></div>
    </div>
    <hr />
    <div v-for="item in registrationList" :key="item.id" class="row">
      <div class="col-3"></div>
      <div class="col-2">{{ item.fullName }}</div>
      <div class="col-2">{{ item.dateRegistr }}</div>
      <div class="col-2">{{ item.temperature }}</div>
      <div class="col-3"></div>
    </div>
    <div>
      <b-button variant="outline-primary" v-b-modal.modal-1
        >Add new user</b-button
      >
      <b-modal
        ref="my-modal"
        size="lg"
        centered
        id="modal-1"
        title="BootstrapVue"
        hide-footer
      >
        <div class="row">
          <div class="col-4 textAlign">Name</div>
          <div class="col-4 textAlign">Date</div>
          <div class="col-4 textAlign">temperature</div>
        </div>
        <b-form @submit="addToRegList" v-if="show">
          <div class="row">
            <div class="col-4 textAlign">
              <b-form-group id="input-group-1" label-for="input-1">
                <b-form-input
                  id="input-1"
                  v-model="form.fullName"
                  type="email"
                  required
                  placeholder="Enter name"
                ></b-form-input>
              </b-form-group>
            </div>
            <div class="col-4 textAlign">
              <b-form-group id="input-group-2" label-for="input-2 ">
                <b-form-input
                  id="input-2"
                  v-model="form.dateRegistr"
                  type="email"
                  required
                  placeholder="Enter date"
                ></b-form-input>
              </b-form-group>
            </div>
            <div class="col-4 textAlign">
              <b-form-input
                id="input-3"
                v-model="form.temperature"
                type="email"
                required
                placeholder="Enter temp"
              ></b-form-input>
            </div>
          </div>
          <b-button
            class="mt-3"
            variant="outline-danger"
            block
            @click="addToRegList"
            >Add user</b-button
          >
        </b-form>
      </b-modal>
    </div>
  </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.min.css";
export default {
  data() {
    return {
      form: {
        fullName: "",
        dateRegistr: "",
        temperature: null,
      },
      show: true,
      registrationList: [
        {
          id: 1,
          fullName: "Slava Tarshevskiy",
          dateRegistr: "01-02-2020",
          temperature: "36.6",
        },
        {
          id: 2,
          fullName: "Freddie Mercury",
          dateRegistr: "01-02-1956",
          temperature: "none",
        },
        {
          id: 3,
          fullName: "some one",
          dateRegistr: "01-02-2020",
          temperature: "37.8",
        },
        {
          id: 4,
          fullName: "some two",
          dateRegistr: "01-02-2020",
          temperature: "37.7",
        },
      ],
    };
  },
  name: "Dashboard",
  props: {
    msg: String,
  },
  created() {
    localStorage.getItem("user") == (null && "null")
      ? this.$router.push("/login")
      : 'this.$router.push("/dashboard");';
  },
  computed: {
    isLoggedIn: function() {
      return this.$store.getters.isLoggedIn;
    },
  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault();

      console.log("this.reg", this.registrationList);
    },
    addToRegList() {
      let res = {
        fullName: this.form.fullName,
        dateRegistr: this.form.dateRegistr,
        temperature: this.form.temperature,
      };
      this.registrationList.push(res);
      this.$refs["my-modal"].hide();
    },
    logoutUser() {
      localStorage.setItem("user", null);
      this.$router.push("/login");
    },
  },
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.textAlign {
  text-align: center;
}
</style>
