<template>
  <main class="container">
    <div class="machine">
      <h1>{{ title }}</h1>
      <div class="slots">
        <p v-for="slot in slots">
          <div class="slot">
            <span>{{ slot.emoji }}</span>
          </div>
        </p>
      </div>
      <button :disabled="finished" @click="play()">PLAY!</button>
      <div class="during" v-if!="finished" lang="fr">Rien ne va plus</div>
      <div class="during" v-if!="hasWon" >verloren</div>
      <div class="during" v-if="hasWon" >Gewonnen</div>
    </div>
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
      for (let i = 0; i < this.slots.length; i++) {
        if (this.slots[i].finished !== true) {
          return true;
        }
      }
      return false;
    },
    hasWon: function () {
      if (this.finished == true) {
        for (let i = 0; i < this.slots.length; i++) {
          if (this.slots[i].emoji === "❓") {
            return false;
          }
          if (this.slots[i].emoji !== this.slots[0].emoji) {
            return false;
          }
        }
      }
      return true;
    },
  },
  watch: {},
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
        if (this.hasWon()) {
          alert("hajajaj");
        } else {
          alert("you suck");
        }
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
