<template>
  <div class="row">
    <input class="name" v-model="user.name">
    <h2>{{ user.points }}</h2>
    <template v-if="user.points > 50">
      <button @click="setPoints(25)" class="reset">Reset to 25</button>
    </template>
    <template v-else>
      <button @click="addPoints(1)">👍1</button>
      <button @click="addPoints(5)">🖐5</button>
    </template>
    <button class="miss" @click="miss">
      <span>Misses:&nbsp;</span>
      <span v-if="!user.misses">0</span>
      <span v-for="i in user.misses" :key="i">💩</span>
    </button>
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
}
.name {
  width: 96px;
  font: inherit;
  font-size: 18px;
  border: none;
  outline: none;
}
button, h2 {
  flex: 1;
}
h2 {
  flex: auto;
  margin: 0;
  align-self: center;
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
  &.reset {
    color: white;
    background: #cc2222;
  }
}
button:active {
  background-color: #6eb9f7;
  background-size: 100%;
  transition: background 0s;
}
.miss {
  display: flex;
  margin-left: 16px;
  line-height: 42px;
  min-width: 104px;
  text-align: left;
}
</style>