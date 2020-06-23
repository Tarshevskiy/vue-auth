<template>
  <div class="home">
    unloggen
    <Dashboard msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import Dashboard from "@/components/Dashboard.vue";

export default {
  name: "Home",
  components: {
    Dashboard,
  },
  created() {
    this.check();
    if (JSON.stringify(localStorage.getItem("user") == "")) {
      this.$router.push("/login");
      console.log("testim2");
    } else {
      this.$router.push("/dashboard");
    }
  },
  methods: {
    check() {
      console.log("localSt(27)", localStorage.getItem("user"));
    },
  },
};
</script>
