<template>
  <div class="row">
    <input class="name" v-model="user.name">
    <h2>{{ user.points }}</h2>
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
</template>

<script>
export default {
  props: {
    user: Object,
  },
  data() {
    return {
    };
  },
  methods: {
    setPoints(points) {
      this.user.points = points;
      this.user.misses = 0;
    },
    addPoints(points) {
      this.setPoints(this.user.points + points);
    },
    miss() {
      if (++this.user.misses >= 3) {
        this.user.points = -1;
        this.user.misses = 0;
      }
    }
  }
}
</script>

<style scoped lang="scss">
.row {
  display: flex;
  margin: 8px 0;
  align-items: center;
}
.name {
  width: 112px;
  margin: 0;
  padding: 0;
  font: inherit;
  font-size: 18px;
  border: none;
  outline: none;
}
button, h2 {
  flex: 1;
}
h2 {
  margin: 0;
  line-height: 32px;
  text-align: left;
}
.bullet {
  flex: 1;
  margin: 0;
  padding: 0 8px;
  border: none;
  border-radius: 8px;
  line-height: 32px;
}
button {
  margin: 0;
  padding: 0 4px;
  border: none;
  border-radius: 8px;
  outline: none;
  font: inherit;
  font-size: 16px;
  background-color: white;
}
.reset {
  color: white;
  background: #cc2222;
}
.winner {
  color: white;
  background: #22cc22;
}
button:active {
  background-color: #6eb9f7;
  background-size: 100%;
  transition: background 0s;
}
.miss {
  display: flex;
  // margin-left: 16px;
  line-height: 42px;
  min-width: 7em;
}
</style>