<template>
  <div class="text-center">
    <div class="actions">
      <div class="numbers-wrapper">
        <div class="numbers">
          <button v-for="position in positions" :key="position.value" :style="positionStyle(position)"
              @click="$emit('commit', position.value)">{{ position.value }}</button>
        </div>
      </div>
      <div class="others">
        <div>
          <h4>Miss</h4>
          <button @click="$emit('commit', 0)">💩</button>
        </div>
        <div>
          <h4>Skip</h4>
          <button @click="$emit('skip')">💤</button>
        </div>
      </div>
    </div>
    <p><i>Record number of <b>points</b> of the round!</i></p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      positions: [
        { value: 1, x: .38, y: .82 },
        { value: 2, x: .62, y: .82 },
        { value: 3, x: .25, y: .60 },
        { value: 4, x: .75, y: .60 },
        { value: 5, x: .13, y: .39 },
        { value: 6, x: .87, y: .39 },
        { value: 7, x: .25, y: .17 },
        { value: 8, x: .75, y: .17 },
        { value: 9, x: .50, y: .17 },
        { value: 10, x: .38, y: .39 },
        { value: 11, x: .62, y: .39 },
        { value: 12, x: .50, y: .60 },
      ],
    };
  },
  methods: {
    positionStyle(i) {
      return { top: `${100 * i.y}%`, left: `${100 * i.x}%` };
    },
  },
}
</script>

<style scoped lang="scss">
.text-center {
  text-align: center;
}
.actions {
  display: flex;
}
button {
  width: 48px;
  height: 48px;
  margin: 0;
  background: linear-gradient(#f4f3d8, #bf9f8f);
  font-size: 22px;
  border: 1px solid #9f7f6f;
  outline: none;
  border-radius: 50%;
}
.numbers-wrapper {
  flex: 3;
}
.numbers {
  position: relative;
  width: 192px;
  height: 192px;
  margin: 0 auto;
  button {
    position: absolute;
    margin: -24px;
    &:active { opacity: 0.6; }
  }
}
.others {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  h4 { margin: 0; }
}
</style>
