<template>
  <di class="container">
    <header class="jumbotron">
      <h3>{{ content }} User dashboard</h3>
    </header>
  </di>
</template>
<script>
// import UserService from "../services/user.service";

export default {
  name: "User",
  data() {
    return {
      content: "",
    };
  },
  mounted() {},
};
</script>