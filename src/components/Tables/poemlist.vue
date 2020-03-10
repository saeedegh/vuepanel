<template>
  <div>
    <md-table v-model="users" :table-header-color="tableHeaderColor">
      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <md-table-cell md-label="ID">{{ item.id }}</md-table-cell>
        <md-table-cell md-label="Name">{{ item.name }}</md-table-cell>
        <md-table-cell md-label="Salary">{{ item.salary }}</md-table-cell>
        <md-table-cell md-label="Country">{{ item.country }}</md-table-cell>
        <md-table-cell md-label="City">{{ item.city }}</md-table-cell>
      </md-table-row>
    </md-table>
    <br />
    <br />
    <div
      v-if="!dataLoaded"
      id="progressEl"
      class="text-center"
      style="z-index:1000"
    >
      <img src="../../assets/img/progress.gif" alt="progressbar" />
    </div>

    <b-table
      id="tbody"
      :items="items"
      :per-page="perPage"
      :current-page="currentPage"
      :fields="fields"
      
    >
      <template v-slot:cell(ردیف)="data">{{
        14 * (currentPage - 1) + data.index + 1
      }}</template>
      <template v-slot:cell(عنوان)="data">{{ data.item.title }}</template>
      <template v-slot:cell(کتاب)="data">
        <div v-if="data.item.book">{{ data.item.book.name }}</div>
        <div v-else></div>
      </template>
      <template v-slot:cell(شاعر)="data">{{
        data.item.poets[0].name
      }}</template>
      <template v-slot:cell(وضعیت)="data">
        <div
          v-if="data.item.ispublished"
          href="#"
          class="btn btn-outline-success"
        >
          منتشر شده
        </div>
        <div v-else href="#" class="btn btn-outline-danger">منتشر نشده</div>
      </template>
      <template v-slot:cell(عملیات)="data">
        <a>
          <i class="material-icons text-secondary text-danger"
            >delete_forever</i
          >
        </a>
        <a
          :href="
            'http://localhost:5969/manage/poem/edit/?item-id=' + data.item._id
          "
        >
          <i class="material-icons text-secondary">edit</i>
        </a>
      </template>

      <template v-slot:cell()="data">
        <i>{{ data.value }}</i>
      </template>
    </b-table>

    <div class="row text-center">
      <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        :per-page="perPage"
        aria-controls="tbody"
      ></b-pagination>
    </div>
  </div>
</template>

<script>
export default {
  name: "ordered-table",
  props: {
    tableHeaderColor: {
      type: String,
      default: ""
    }
  },
  components: {},
  data() {
    return {
      selected: [],
      users: [
        {
          id: 1,
          name: "Dakota Rice",
          salary: "$36,738",
          country: "Niger",
          city: "Oud-Turnhout"
        },
        {
          id: 2,
          name: "Minerva Hooper",
          salary: "$23,738",
          country: "Curaçao",
          city: "Sinaai-Waas"
        },
        {
          id: 3,
          name: "Sage Rodriguez",
          salary: "$56,142",
          country: "Netherlands",
          city: "Overland Park"
        },
        {
          id: 4,
          name: "Philip Chaney",
          salary: "$38,735",
          country: "Korea, South",
          city: "Gloucester"
        }
      ],
      items: [],
      fields: ["ردیف", "عنوان", "کتاب", "شاعر", "وضعیت", "عملیات"],
      dataLoaded: false,
      response: {},
      currentPage: 1,
      perPage: 14
    };
  },
  computed: {
    rows() {
      return this.response.totalCount;
    }
  },
  mounted() {
    this.dataLoaded = false;
    this.$http
      .get(`http://localhost:5969/manage/poem/getPagedPoems`)
      .then(info => info.json())
      .then(response => {
        this.response = response;
        this.items = response.items;
        this.dataLoaded = true;
      });
  }
};
</script>
<style scoped>
.md-card img {
  width: 50px;
  height: 50px;
}
</style>
