<template>
  <b-form-select
    id="filters"
    v-model="selectedFilter"
    :options="filterOptions"
    placeholder="Search.."
  ></b-form-select>
</template>

<script>
export default {
  props: ['filterOptions', 'isResetSelect'],
  data() {
    return {
      selectedFilter: null,
    }
  },
  watch: {
    selectedFilter() {
      this.$emit('selected', this.selectedFilter)
    },
    isResetSelect: {
      immediate: true,
      handler(val) {
        if (val) {
          this.selectedFilter = null
        }
      },
    },
  },
}
</script>

<style></style>
