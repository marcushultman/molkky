<template>
  <div>
    <h1>Mölkky</h1>
    <div class="edit">
      <button @click="add">➕</button>
      <button @click="remove">➖</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    users: Array,
  },
  methods: {
    add() {
      this.users.push({ name: 'Name ' + (this.users.length + 1), points: 0, misses: 0 });
    },
    remove() {
      this.users.splice(-1);
    },
  }
}
</script>

<style lang="scss" scoped>
h1 {
  margin: 32px 0 8px 0;
}
button {
  font: inherit;
  font-size: 24px;
}
</style>