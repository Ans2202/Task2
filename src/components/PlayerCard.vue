<template>
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
  </template>
  
  <script>
  export default {
    props: {
      player: {
        type: Object,
        required: true,
      },
    },
    methods: {
      getRole(roleId) {
        const roles = ['Batsman', 'All-Rounder', 'Bowler'];
        return roles[roleId - 1] || 'Unknown';
      }
    }
  };
  </script>
  
  <style scoped>
  .card {
    background-color: white;
    border-radius: 10px;
    box-shadow: 0px 5px 10px rgba(0, 0, 0, 0.1);
    padding: 15px;
    margin-bottom: 20px;
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
  </style>
  