<template>
  <main class="container">
    <div class="machine">
      <h1 class="shadow">{{ title }}</h1>
      <div class="slots" :class="{ spinning: finished == false }">
        <div
          :class="{ won: hasWon == true }"
          class="slot shadow"
          v-for="slot in slots"
          :key="slot.emoji"
        >
          <span>{{ slot.emoji }}</span>
        </div>
      </div>
      <div class="containerButtons">
        <button
          class="shadow extraText"
          :disabled="finished == false"
          @click="play()"
        >
          Spelen
        </button>
        <p class="shadow extraText" v-if="finished == false" lang="fr">
          Rien ne va plus
        </p>
        <p class="shadow extraText" v-if="hasWon == false && finished == true">
          Verloren
        </p>
        <p class="shadow extraText" v-if="hasWon == true && finished == true">
          Gewonnen
        </p>
      </div>
    </div>
    <audio
      preload="auto"
      volume="0.1"
      ref="tick"
      src="185611__kubawolanin__metal-tick-1.wav"
      loop
      hidden
    ></audio>
    <audio
      preload="auto"
      ref="ding"
      src="411088__inspectorj__bell-candle-damper-ah4n.wav"
      hidden
    ></audio>
  </main>
</template>
<script>
const emoji = ["🍉", "🍌", "🍍", "🍑"];
export default {
  data() {
    return {
      title: "THE EPIC SLOT MACHINE",
      slots: [
        {
          emoji: "❓",
          finished: true,
        },
        {
          emoji: "❓",
          finished: true,
        },
        {
          emoji: "❓",
          finished: true,
        },
      ],
    };
  },
  computed: {
    finished: function () {
      function isFinished(element) {
        return element.finished === true;
      }
      return this.slots.every(isFinished);
    },
    hasWon: function () {
      function isFinished(element) {
        return element.finished === true;
      }
      if (this.slots.every(isFinished)) {
        for (let j = 0; j < this.slots.length; j++) {
          if (this.slots[j].emoji === "❓") {
            return false;
          }
          if (this.slots[j].emoji !== this.slots[0].emoji) {
            return false;
          }
        }
        return true;
      }
      return false;
    },
  },
  watch: {
    finished(finished) {
      finished ? this.$refs.tick.pause() : this.$refs.tick.play();
    },
  },

  methods: {
    rotate(slot) {
      // 1. zet finished op false
      slot.finished = false;
      // 2. draaien
      let i = 0;
      const int = setInterval(() => {
        slot.emoji = emoji[i];
        i++;
        if (i >= emoji.length) {
          i = 0;
        }
      }, 120);
      // 3. elk slot 'draait' voor een willekeurige tijd
      setTimeout(() => {
        clearInterval(int);
        slot.finished = true;
        this.$refs.ding.currentTime = 0;
        this.$refs.ding.play();
      }, 2000 + Math.random() * 3000);
    },
    play() {
      this.slots.forEach(this.rotate);
    },
  },
};
</script>
<style lang="scss">
@import "./assets/base.css";
</style>
