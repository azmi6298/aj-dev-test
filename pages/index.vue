<template>
  <b-container>
    <h1 class="my-4">Example with Search and Filter</h1>
    <div class="d-flex my-4 align-bottom">
      <div role="group">
        <label for="searchKeyword">Search</label>
        <b-input-group>
          <b-form-input
            id="searchKeyword"
            placeholder="Search.."
          ></b-form-input>
          <b-input-group-append>
            <b-button variant="primary" @click="search">
              <BIconSearch></BIconSearch>
            </b-button>
          </b-input-group-append>
        </b-input-group>
      </div>
      <div role="group" class="ml-3">
        <label for="filters">Gender</label>
        <b-form-select
          id="filters"
          v-model="selectedFilter"
          :options="filterOptions"
          placeholder="Search.."
        ></b-form-select>
      </div>
      <b-button variant="outline-secondary" class="ml-3" @click="resetFilter">
        Reset Filter
      </b-button>
    </div>
    <b-table hover :items="users" :fields="fields"></b-table>
    <div class="overflow-auto">
      <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        align="right"
      ></b-pagination>
    </div>
  </b-container>
</template>

<script>
import { BIconSearch } from 'bootstrap-vue'

const fields = [
  {
    label: 'Username',
    key: 'username',
  },
  {
    label: 'Name',
    key: 'name',
    sortable: true,
  },
  {
    label: 'Email',
    key: 'email',
    sortable: true,
  },
  {
    label: 'Gender',
    key: 'gender',
    sortable: true,
  },
  {
    label: 'Registered Date',
    key: 'registered',
    sortable: true,
  },
]

const filterOptions = [
  { value: null, text: 'Please select an option' },
  { value: 'male', text: 'Male' },
  { value: 'female', text: 'Female' },
]

export default {
  name: 'IndexPage',
  components: {
    BIconSearch,
  },
  data() {
    return {
      users: [],
      searchKeyword: '',
      filterOptions,
      selectedFilter: null,
      fields,
      rows: 100,
      currentPage: 1,
    }
  },
  async fetch() {
    let query = ''

    if (this.selectedFilter !== null) {
      query += `&gender=${this.selectedFilter}`
    }

    const { results: res } = await this.$axios.$get(
      `https://randomuser.me/api/?page=${this.currentPage}&results=5${query}`
    )

    this.users = res.map((data) => {
      return {
        username: data.login.username,
        name: `${data.name.first} ${data.name.last}`,
        email: data.email,
        gender: data.gender,
        registered: data.registered.date,
      }
    })
  },
  watch: {
    selectedFilter() {
      this.refresh()
    },
    currentPage() {
      this.refresh()
    },
  },
  methods: {
    refresh() {
      this.$fetch()
    },
    search() {
      console.log('search')
    },
    resetFilter() {
      this.selectedFilter = null
    },
  },
}
</script>
