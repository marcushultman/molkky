<template>
  <div class="row">
    <div class="flex">
      <input class="name" v-model="user.name">
      <input class="points" v-model.number="user.points">
      <template v-if="user.points > 50">
        <button @click="setPoints(25)" class="bullet reset">Reset to 25</button>
      </template>
      <template v-else-if="user.points == 50">
        <div class="bullet winner">Winner</div>
      </template>
      <template v-else>
        <button @click="addPoints(1)">👍1</button>
        <button @click="addPoints(5)">🖐5</button>
        <button class="bullet miss" @click="miss">
          <span>Misses:&nbsp;</span>
          <span v-if="!user.misses">-</span>
          <span v-for="i in user.misses" :key="i">💩</span>
        </button>
      </template>
    </div>
    <div class="flex indicators" @click="++turn">
      <span v-for="i in turn" :key="i" class="turn-indicator" @click.stop="--turn"/>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    user: Object,
    active: Boolean,
  },
  data() {
    return {
      turn: 0,
    };
  },
  watch: {
    active(isActive, wasActive) {
      if (isActive && !wasActive) {
        ++this.turn;
      }
    },
  },
  methods: {
    setPoints(points) {
      this.user.points = points;
      this.user.misses = 0;
    },
    addPoints(points) {
      this.setPoints(this.user.points + points);
      this.$emit('turn');
    },
    miss() {
      if (++this.user.misses === 3) {
        this.user.points = -1;
        this.user.misses = 0;
      }
      this.$emit('turn');
    }
  }
}
</script>

<style scoped lang="scss">
.row {
  padding: 4px 0;
}
.flex {
  display: flex;
  align-items: center;
}
input {
  margin: 0;
  padding: 0;
  color: inherit;
  background: inherit;
  border: none;
  outline: none;
}
.name {
  width: 112px;
  font: inherit;
  font-size: 18px;
}
.points {
  flex: 1;
  width: 0;
  font-size: 22px;
  font-weight: bold;
}
button {
  flex: 1;
  color: inherit;
  background: inherit;
  margin: 0;
  padding: 0 4px;
  border: none;
  border-radius: 8px;
  outline: none;
  font: inherit;
  font-size: 16px;
}
button:active {
  background-color: #6eb9f7;
  background-size: 100%;
  transition: background 0s;
}
.bullet {
  flex: 1;
  margin: 0;
  padding: 0 8px;
  border: none;
  border-radius: 8px;
  line-height: 32px;
}
.reset {
  margin: 5px 0;
  color: white;
  background: #cc2222;
}
.winner {
  margin: 5px 0;
  color: white;
  background: #22cc22;
}
.miss {
  display: flex;
  line-height: 42px;
  min-width: 7em;
}
.indicators {
  padding: 8px 0;
  min-height: 4px;
}
.turn-indicator {
  width: 32px;
  height: 4px;
  background: #446688;
  border-radius: 2px;
  margin-right: 8px;
}
</style>