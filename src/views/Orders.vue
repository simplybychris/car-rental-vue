<template>
  <b-container fluid>
    <!-- User Interface controls -->
    <!-- Main table element -->
    <h2>Orders</h2>
    <b-table
      :items="items"
      :fields="fields"
      :current-page="currentPage"
      :per-page="perPage"
      :filter="filter"
      :filter-included-fields="filterOn"
      :sort-by.sync="sortBy"
      :sort-desc.sync="sortDesc"
      :sort-direction="sortDirection"
      stacked="md"
      show-empty
      small
      @filtered="onFiltered"
    >
      <template #cell(name)="row">
        {{ row.value.first }} {{ row.value.last }}
      </template>

      <template #row-details="row">
        <b-card>
          <ul>
            <li v-for="(value, key) in row.item" :key="key">
              {{ key }}: {{ value }}
            </li>
          </ul>
        </b-card>
      </template>
    </b-table>

    <!-- Info modal -->
  </b-container>
</template>
<script>
import axios from "axios";
export default {
  name: "Orders",
  data() {
    return {
      content: "Orders",
      items: [],
      fields: [
        {
          key: "returnSalonId",
          label: "returnSalonId",
          sortable: true,
          class: "text-center",
        },
        {
          key: "statusId",
          label: "statusId",
          sortable: true,
        },
        {
          key: "carsId",
          label: "carsId",
          sortable: true,
        },
      ],
    };
  },
  async created() {
    try {
      const res = await axios.get(process.env.VUE_APP_SALON_API + "/orders");
      console.log(res.data);
      this.items = res.data;
    } catch (e) {
      console.error(e);
    }
  },
  computed: {
    sortOptions() {
      // Create an options list from our fields
      return this.fields
        .filter((f) => f.sortable)
        .map((f) => {
          return { text: f.label, value: f.key };
        });
    },
  },
  mounted() {
    // Set the initial number of items
    this.totalRows = this.items.length;
  },
  methods: {
    resetInfoModal() {
      this.infoModal.title = "";
      this.infoModal.content = "";
    },
    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length;
      this.currentPage = 1;
    },
  },
};
</script>