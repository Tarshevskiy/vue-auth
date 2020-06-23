<template>
  <div>
    <form class="login" @submit.prevent="login">
      <h1>Sign in</h1>
      <label>Email</label>
      <input required v-model="email" type="email" placeholder="Name" />
      <label>Password</label>
      <input
        required
        v-model="password"
        type="password"
        placeholder="Password"
      />
      <hr />
      <button type="submit" @click="handleSubmit" style="margin-right: 30px">
        Login
      </button>
      <button type="submit" @click="redirect">Sign up</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      //   emptyFields: false,
    };
  },
  created() {
    if (localStorage.getItem("user") != (null || "null")) {
      this.$router.push("/dashboard");
    }
  },
  methods: {
    login: function() {
      let email = this.email;
      let password = this.password;
      this.$store
        .dispatch("login", { email, password })
        .then(() => this.$router.push("/dashboard"))
        .catch((err) => console.log(err));
    },
    redirect: function() {
      this.$router.push("/register");
    },
    handleSubmit(e) {
      e.preventDefault();
      if (this.password.length > 0 && this.email.length > 0) {
        this.$http
          .post("http://localhost:3000/login", {
            email: this.email,
            password: this.password,
          })
          .then((response) => {
            let is_admin = response.data.user.is_admin;
            localStorage.setItem("user", JSON.stringify(response.data.user));
            localStorage.setItem("jwt", response.data.token);

            if (localStorage.getItem("jwt") != null) {
              this.$emit("loggedIn");
              if (this.$route.params.nextUrl != null) {
                this.$router.push(this.$route.params.nextUrl);
              } else {
                if (is_admin == 1) {
                  this.$router.push("admin");
                } else {
                  this.$router.push("dashboard");
                }
              }
            }
          })
          .catch(function(error) {
            console.error(error.response);
          });
      } else {
        // this.emptyFields = true;
        alert("Empty fields");
      }
    },
  },
};
</script>
