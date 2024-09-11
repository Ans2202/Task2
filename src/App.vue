<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg bg-gradient">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Sportz Interactive</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <select class="form-select team-select" v-model="selectedTeam" @change="filterByTeam">
              <option value="ALL">ALL</option>
              <option value="IND">India</option>
              <option value="PAK">Pakistan</option>
              <option value="AUS">Australia</option>
              <option value="ENG">England</option>
            </select>
          </ul>
          <form class="d-flex" role="search">
            <input
              class="form-control me-2 search-input"
              type="search"
              placeholder="Search player"
              v-model="searchQuery"
            />
            <button class="btn btn-custom" type="button" @click="searchPlayers">
              Search
            </button>
          </form>
        </div>
      </div>
    </nav>

    <div class="container">
      <h1 class="heading">Cricket Players</h1>
      <div v-if="players.length">
        <div v-for="player in filteredPlayers" :key="player.name" class="mb-3">
          <div class="card">
            <img :src="player.image" class="card-img-top" alt="Player Image" v-if="player.image"/>
            <div class="card-body">
              <h5 class="card-title">{{ player.name }}</h5>
              <p class="card-text">Matches: {{ player.matches }}</p>
              <p class="card-text">Role: {{ getRole(player.role) }}</p>
              <p class="card-text">Runs: {{ player.runs }}</p>
              <p class="card-text">50s: {{ player["50s"] }}</p>
              <p class="card-text">100s: {{ player["100s"] }}</p>
              <p class="card-text">Highest Score: {{ player.highest_score }}</p>
              <p class="card-text">Best Bowling Figures: {{ player.best_bowling_figures || 'N/A' }}</p>
              <p class="card-text">Team: {{ player.team_name }}</p>
            </div>
          </div>
        </div>
      </div>
      <div v-else>
        <p>Loading players...</p>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      players: [],
      searchQuery: "",
      selectedTeam: "ALL",
    };
  },
  computed: {
    filteredPlayers() {
      return this.players.filter(player => {
        const matchesQuery = player.name.toLowerCase().includes(this.searchQuery.toLowerCase());
        const matchesTeam = this.selectedTeam === "ALL" || player.team_name === this.selectedTeam;
        return matchesQuery && matchesTeam;
      });
    }
  },
  created() {
    fetch('/players.json')
      .then(response => response.json())
      .then(data => {
        this.players = data.originalPlayers;
      })
      .catch(error => console.error('Error fetching player data:', error));
  },
  methods: {
    getRole(roleId) {
      const roles = ['Batsman', 'All-Rounder', 'Bowler'];
      return roles[roleId - 1] || 'Unknown';
    },
    searchPlayers() {
      // Triggered by the Search button, handled by computed properties
    },
    filterByTeam() {
      // Triggered by the team select change, handled by computed properties
    }
  }
};
</script>

<style scoped>
#app {
  font-family: 'Roboto', sans-serif;
  background: #f5f5f5;
  color: #333;
}

.navbar {
  background: linear-gradient(to right, #007bff, #6610f2);
  color: white;
}

.navbar .navbar-brand {
  color: white;
  font-size: 1.5rem;
  font-weight: bold;
}

.navbar .navbar-brand:hover {
  color: #ddd;
}

.navbar .form-select.team-select {
  background-color: #f5f5f5;
  border: none;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
}

.search-input {
  border-radius: 20px;
  border: 1px solid #ddd;
  padding-left: 15px;
  padding-right: 15px;
  font-size: 1rem;
}

.btn-custom {
  background-color: #28a745;
  border-color: #28a745;
  color: white;
  border-radius: 20px;
  padding: 5px 20px;
  transition: background-color 0.3s ease;
}

.btn-custom:hover {
  background-color: #218838;
  border-color: #218838;
}

.heading {
  text-align: center;
  font-size: 2rem;
  margin: 20px 0;
}

.card {
  background-color: white;
  border-radius: 10px;
  box-shadow: 0px 5px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease-in-out;
  padding: 15px;
}

.card:hover {
  transform: translateY(-10px);
}

.card-img-top {
  border-radius: 10px;
  height: 150px;
  width: 100%;
  object-fit: cover;
}

.card-title {
  font-weight: bold;
  color: #333;
}

.card-text {
  color: #555;
}

/* Responsive Adjustments */
@media (max-width: 768px) {
  .heading {
    font-size: 1.5rem;
  }
}
</style>
