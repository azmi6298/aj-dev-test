<template>
  <b-container>
    <h1 class="my-4">Example with Search and Filter</h1>
    <div class="d-flex my-4 align-bottom">
      <div role="group">
        <label for="searchKeyword">Search</label>
        <b-input-group>
          <b-form-input
            id="searchKeyword"
            v-model="searchKeyword"
            placeholder="Search.."
          ></b-form-input>
          <b-input-group-append>
            <b-button variant="primary" @click="searchForKeyword">
              <BIconSearch></BIconSearch>
            </b-button>
          </b-input-group-append>
        </b-input-group>
      </div>
      <div role="group" class="ml-3">
        <label for="filters">Gender</label>
        <BasicSelect
          :filter-options="filterOptions"
          :is-reset-select="isResetSelect"
          @selected="handleSelectGender"
        />
      </div>
      <b-button variant="outline-secondary" class="ml-3" @click="resetFilter">
        Reset Filter
      </b-button>
    </div>
    <BasicTable
      :items="users"
      :fields="fields"
      :rows="rows"
      @pageChanged="handleChangePage"
    />
  </b-container>
</template>

<script>
import { BIconSearch } from 'bootstrap-vue'
import BasicTable from '../components/table/BasicTable.vue'
import BasicSelect from '~/components/select/BasicSelect.vue'

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
    BasicSelect,
    BasicTable,
  },
  data() {
    return {
      users: [],
      searchKeyword: '',
      selectedFilter: null,
      isResetSelect: false,
      filterOptions,
      fields,
      rows: 100,
      targetPage: 1,
    }
  },
  async fetch() {
    let query = ''

    if (this.selectedFilter !== null) {
      query += `&gender=${this.selectedFilter}`
    }

    if (this.searchKeyword !== '') {
      query += `&keyword=${this.searchKeyword}`
    }

    const { results: res } = await this.$axios.$get(
      `https://randomuser.me/api/?page=${this.targetPage}&results=5${query}`
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
  methods: {
    refresh() {
      this.$fetch()
    },
    searchForKeyword() {
      this.$fetch()
    },
    resetFilter() {
      this.isResetSelect = true
    },
    handleSelectGender(value) {
      this.selectedFilter = value
      this.isResetSelect = false
      this.refresh()
    },
    handleChangePage(value) {
      this.targetPage = value
      this.refresh()
    },
  },
}
</script>
