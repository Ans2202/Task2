<template>
    <div class="filter-container">
      <select v-model="selectedTeam" @change="applyFilters">
        <option value="ALL">All Teams</option>
        <option value="IND">India</option>
        <option value="PAK">Pakistan</option>
        <option value="AUS">Australia</option>
        <option value="ENG">England</option>
      </select>
      <input type="text" v-model="searchQuery" @input="applyFilters" placeholder="Search by player name" />
    </div>
  </template>
  
  <script>
  export default {
    props: {
      searchQuery: String,
      selectedTeam: String
    },
    methods: {
      applyFilters() {
        this.$emit('filter-changed', { searchQuery: this.searchQuery, selectedTeam: this.selectedTeam });
      }
    }
  };
  </script>
  
  <style scoped>
  .filter-container {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
  }
  select, input {
    padding: 8px;
    margin-right: 10px;
  }
  </style>
  