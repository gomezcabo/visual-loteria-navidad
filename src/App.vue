<template>
  <div class="w-full flex flex-wrap border-1 border-black">
    <div
      v-for="number in numbers"
      class="w-1 h-1"
      :class="{
        'bg-green-900': number && number !== 200,
        'rounded-full': number,
        'bg-pink-100': number === 200,
        'z-0': number === 200,
        'z-10': number !== 200,
      }"
      :style="{
        transform: `scale(${1 + (20 * number) / 4_000_000})`,
        opacity: number > 2000 ? 0.5 : 1,
      }"
    ></div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

const prizes = [
  { n: 1, prize: 4_000_000 },
  { n: 1, prize: 1_250_000 },
  { n: 1, prize: 500_000 },
  { n: 2, prize: 200_000 },
  { n: 8, prize: 60_000 },
  { n: 2, prize: 20_000 },
  { n: 1794, prize: 1_000 },
];

export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {},
  computed: {
    numbers() {
      const result = Array(100_000).fill(0);

      const winner = this.getRandomNumber(result);
      result[winner] = 4_000_000;

      for (let i = 1; i < prizes.length; i++) {
        const prize = prizes[i];
        for (let j = 0; j < prize.n; j++) {
          const number = this.getRandomNumber(result);
          result[number] = prize.prize;
        }
      }

      for (let i = 0; i < result.length; i++) {
        if (i % 10 === winner % 10) {
          result[i] += 200;
        }
      }
      console.log(result);

      return result;
    },
  },
  methods: {
    getRandomNumber(results) {
      let number = Math.floor(Math.random() * 100_000);
      while (results[number] !== 0) {
        number = Math.floor(Math.random() * 100_000);
      }
      return number;
    },
  },
};
</script>
