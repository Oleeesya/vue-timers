<template>
  <div class="container">
    <div v-for="item of timers" v-bind:key="item">
      <Timer />
    </div>
    <div class="add" type="button" @click="addTimer">
      <svg
        width="20"
        height="20"
        viewBox="0 0 20 20"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <rect x="8.5" width="3" height="20" fill="#9E9E9E" />
        <rect
          y="11.5"
          width="3"
          height="20"
          transform="rotate(-90 0 11.5)"
          fill="#9E9E9E"
        />
      </svg>
    </div>
  </div>
</template>

<script>
import Timer from "./components/Timer.vue";

export default {
  name: "app",
  components: {
    Timer,
  },
  props: ["Timer"],
  data() {
    return {
      timers: 1,
    };
  },
  methods: {
    addTimer() {
      this.timers++;
    },
  },
};
</script>

<style>
.body {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  background: #e5e5e5;
}

.container {
  max-width: 775px;
  margin: 75px auto 0;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(225px, max-content));
  grid-gap: 45px 50px;
}

[v-cloak] {
  display: none;
}

@media (max-width: 790px) {
  .container {
    max-width: 500px;
  }
}

@media (max-width: 516px) {
  .container {
    max-width: 225px;
  }
}
</style>
