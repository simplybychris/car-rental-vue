<template>
  <div class="container">
    <header class="jumbotron">
      <h3>Book cars {{ this.$route.params.id }}</h3>
      <b-form inline>
        <div class="w-50">
          <label class="text-left" for="datepicker_from">Pick-up date</label>
          <b-form-datepicker
            id="datepicker_from"
            v-model="selectedFrom"
            class="mb-2"
          ></b-form-datepicker>
        </div>
        <div class="w-50">
          <label class="text-left" for="datepicker_to">Drop-off date</label>
          <b-form-datepicker
            id="datepicker_to"
            v-model="selectedTo"
            class="mb-2"
          ></b-form-datepicker>
        </div>
      </b-form>
      <div class="w-50 mb-2">
        <label for="salon_picker">Salon to return</label>
        <b-form-select
          id="salon_picker"
          w-25
          v-model="selectedSalon"
          :options="salons"
        ></b-form-select>
      </div>
      <b-button variant="success" size="lg">Reserve</b-button>
    </header>
    <b-calendar :date-info-fn="dateClass" block locale="en-US"></b-calendar>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "Book",
  data() {
    return {
      carId: this.$route.params.id,
      selectedFrom: null,
      selectedTo: null,
      selectedSalon: "select salon to return",
      salons: [],
      items: [],
    };
  },
  mounted() {},
  async created() {
    try {
      const res = await axios.get(process.env.VUE_APP_AUTH_API + "/salons");
      let salonsList = res.data;
      this.salons = salonsList.map((item) => {
        let obj = {};
        console.log(obj);
        obj["value"] = item._id;
        obj["text"] = item.Name;
        return obj;
      });
    } catch (e) {
      console.error(e);
    }
  },
  methods: {
    dateClass(ymd) {
      let command = "";
      // console.log("date: ", day, ", ymd = ", ymd);
      let array = this.$store.state.auth.carsDate;
      array.forEach(function (item) {
        // console.log("item date from: ", item.date_from);
        let from = item.date_from.toString().split("T")[0];
        let to = item.date_to.toString().split("T")[0];
        if (ymd >= from && ymd <= to) {
          command = "table-warning";
        }
      });
      return command;
      // return ymd >= item.date_from ? "table-info" : "";

      // console.log(ymd);
    },
    // async initializeData() {
    //   try {
    //     const res = await axios.get(
    //       process.env.VUE_APP_SALON_API + "/cars/" + this.$route.params.id
    //     );
    //     console.log("res data: ", res.data);
    //     this.items = await res.data;
    //     console.log("items: ", this.items);
    //   } catch (e) {
    //     console.error(e);
    //   }
    // },
  },
};
</script>