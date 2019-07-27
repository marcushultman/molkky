<template>
  <div id="app">
    <h1>Mölkky</h1>
    <button class="top-button top-button-left" @click="resetGame">🆕</button>
    <button class="top-button top-button-right" @click="addPlayer">🙋‍♀️</button>
    <template v-if="players.length">
      <div class="player-count-label">{{ players.length }} players</div>
      <transition-group name="player-columns" tag="div" class="player-columns">
        <PlayerColumn v-for="playerScore in playerScores" :key="playerScore.player.id"
            :playerScore="playerScore"
            :active="playerScore.player == currentPlayer"/>
      </transition-group>
    </template>
    <hr>
    <div v-if="winner" class="end-status winner">Winner: {{ winner.name }}</div>
    <div v-else-if="!currentPlayer && players.length" class="end-status">No one won 🤷‍♀️</div>
    <h3 v-else-if="!currentPlayer">Waiting for players...</h3>
    <template v-else>
      <h3>Next up: {{ currentPlayer.name }}</h3>
      <CurrentPlayerActions @commit="commitRound" @skip="skipRound"/>
    </template>
  </div>
</template>

<script>
import CurrentPlayerActions from './components/CurrentPlayerActions.vue'
import PlayerColumn from './components/PlayerColumn.vue'

function playerScore(player) {
  return player.rounds
      .filter(points => points !== Infinity)
      .reduce((total, points) => total + points, 0);
}
function playerLost(player) {
  const lastThree = player.rounds
      .filter(points => points !== Infinity)
      .slice(-3);
  return lastThree.length === 3 && lastThree.every(points => points === 0);
}
function comparePlayers(round) {
  return (lhs, rhs) => {
    if (playerLost(lhs)) {
      return Infinity;
    } else if (playerLost(rhs)) {
      return -Infinity;
    }
    return lhs.rounds[round] -rhs.rounds[round];
  };
}

export default {
  name: 'app',
  components: {
    CurrentPlayerActions,
    PlayerColumn,
  },
  data() {
    return {
      players: [],
    };
  },
  computed: {
    playerScores() {
      return this.players.reduce((players, player) => [
        ...players, { player, score: playerScore(player), lost: playerLost(player) }
      ], []);
    },
    round() {
      return Math.max(0, ...this.players.map(player => player.rounds.length));
    },
    currentPlayer() {
      const players = this.playerScores.filter(({ lost }) => !lost).map(({ player }) => player);
      return players.find(player => player.rounds.length < this.round) || players[0] || null;
    },
    winner() {
      const playerScore = this.playerScores.find(({ score }) => score === 50);
      return playerScore ? playerScore.player : null;
    },
  },
  methods: {
    resetGame() {
      this.players = [];
    },
    addPlayer() {
      this.players.push({
        id: this.players.length,
        name: `Player ${this.players.length + 1}`,
        rounds: Array(Math.max(0, this.round - 1)).fill(Infinity),
      });
    },
    commitRound(points) {
      this.currentPlayer.rounds.push(points);
      this.correctScores();
      this.sortPlayers();
    },
    skipRound() {
      this.commitRound(Infinity);
    },
    correctScores() {
      this.playerScores
          .filter(({ score }) => score > 50)
          .forEach(({ score, player }) => {
            const last = player.rounds.length - 1;
            player.rounds[last] = 25 - (score - player.rounds[last]);
          });
    },
    sortPlayers() {
      const prevRound = this.round - 1;
      if (prevRound >= 0) {
        this.players.sort(comparePlayers(prevRound));
      }
    },
  },
}
</script>

<style>
body {
  margin: 0;
}
</style>
<style scoped lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 8px;
  overflow: visible;
}
h1, h3 {
  text-align: center;
}
h1 {
  margin: 24px 0 8px 0;
}
.top-button {
  position: absolute;
  top: 0;
  width: 40px;
  height: 40px;
  font-size: 40px;
  line-height: 40px;
  margin: 16px;
  padding: 0;
  border: none;
  outline: none;
  background: inherit;
}
.top-button-left {
  left: 0;
}
.top-button-right {
  right: 0;
}
.player-count-label {
  font-weight: bold;
}
.player-columns {
  display: flex;
  overflow-x: scroll;
  padding: 8px;
  margin: 0 -8px;
}
.player-columns-move {
  transition: transform 200ms;
}
.end-status {
  text-align: center;
  font-size: 32px;
  font-weight: bold;
  border-radius: 8px;
  &.winner { background: lightgreen; }
}
</style>
