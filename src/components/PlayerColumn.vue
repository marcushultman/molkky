<template>
  <div class="player-column flex-column" :class="{ active, lost }">
    <div class="flex">
      <input v-model="player.name">
    </div>
    <hr>
    <div v-if="!player.rounds.length" class="round">&nbsp;</div>
    <div v-for="(points, index) in player.rounds" :key="index" class="round">
      {{ pointsString(points) }}
    </div>
    <div class="filler"/>
    <hr>
    <div class="round total">{{ lost ? 'Lost' : score }}</div>
  </div>
</template>

<script>
export default {
  props: {
    playerScore: Object,
    active: Boolean,
  },
  computed: {
    player() {
      return this.playerScore.player;
    },
    score() {
      return this.playerScore.score;
    },
    lost() {
      return this.playerScore.lost;
    },
    pointsString() {
      return points => points === Infinity ? '-' : points < 0 ? '🔙' : !points ? '💩' : points;
    },
  },
}
</script>

<style scoped lang="scss">
.player-column {
  min-width: 22.5%;
  max-width: 22.5%;
  margin: 0 -.5px;
  text-align: center;
  border: 1px solid black;
  &.active { background: #eee; }
  &.lost { background: #dcc; }
}
.flex {
  display: flex;
  align-items: center;
}
.flex-column {
  display: flex;
  flex-direction: column;
}
input {
  width: 0;
  flex: 1;
  margin: 4px;
  padding: 0;
  color: inherit;
  background: inherit;
  border: none;
  outline: none;
  font: inherit;
  font-size: 16px;
}
hr {
  margin: 0;
}
button {
  display: block;
  color: inherit;
  background: inherit;
  margin: 0;
  padding: 0;
  border: none;
  border-radius: 8px;
  outline: none;
  font: inherit;
  font-size: 20px;
  &:active {
    background-color: #6eb9f7;
    background-size: 100%;
    transition: background 0s;
  }
}
.round {
  line-height: 24px;
  &.total {
    font-weight: bold;
  }
}
.filler {
  flex: 1;
}
</style>
