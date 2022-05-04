<template>
  <div>
    <b-table hover :items="items" :fields="fields"></b-table>
    <div class="overflow-auto">
      <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        align="right"
      ></b-pagination>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: [],
    },
    fields: {
      type: [],
    },
  },
  data() {
    return {
      rows: 100,
      currentPage: 1,
    }
  },
  watch: {
    currentPage() {
      this.$emit('pageChanged', this.currentPage)
    },
  },
}
</script>

<style></style>
