<template>
  <div class="container">
    <header class="jumbotron">
      <h3>{{ content }}</h3>
    </header>
  </div>
</template>

<script>
export default {
  name: "User",
  data() {
    return {
      fields: [],
      items: [
        {
          pickupSalonId: "Dickerson",
          returnSalonId: "Macdonald",
          carsId: 6235236326326724,
        },
        { age: 21, first_name: "Larsen", last_name: "Shaw" },
        { age: 89, first_name: "Geneva", last_name: "Wilson" },
        { age: 38, first_name: "Jami", last_name: "Carney" },
      ],
      content: "Transport",
    };
  },
  mounted() {},
};
</script>