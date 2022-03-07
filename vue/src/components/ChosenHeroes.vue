<template>
  <div>
    <div class="selectionContainer">
      <select
        v-model="chosenHero"
        :disabled="chosenHeroesSelected.length === 3"
      >
        <!-- placeholder value -->
        <option :value="null">Select a hero</option>

        <!-- available heroes -->
        <option
          v-for="hero in heroes"
          :key="hero.id"
          :value="{ id: hero.id, name: hero.name }"
          :disabled="chosenHeroesSelected.find((h) => hero.name === h.name)"
        >
          {{ hero.name }}
        </option>
      </select>
      <span>&nbsp;</span>
      <div class="btnContainer">
        <button
          @click="addHero(chosenHero)"
          :disabled="chosenHeroesSelected.length === 3"
        >
          Add Hero
        </button>
        <button
          @click="launch"
          class="launchBtn"
          v-if="chosenHeroesSelected.length === 3"
        >
          Launch Mission
        </button>
      </div>
    </div>
    <br />
    <h3>Chosen Heroes</h3>
    <div class="chosen-heroes">
      <div v-for="(hero, i) in chosenHeroesSelected" :key="hero.id">
        <strong>Slot {{ i + 1 }}:</strong>
        <Hero :hero="hero" @removeHero="removeHero(hero)" />
      </div>
    </div>
  </div>
</template>

<script>
import Hero from "./Hero";

export default {
  components: {
    Hero,
  },
  props: ["heroes", "chosenHeroesSelected"],
  data() {
    return {
      chosenHero: null,
      launchedMission: false,
    };
  },
  methods: {
    addHero(data) {
      this.$emit("update:heroAdded", { id: data.id, name: data.name });
      this.chosenHero = null;
    },

    removeHero(hero) {
      this.$emit("update:heroRemoved", hero);
    },
    launch() {
      this.launchedMission = true;
      alert("Launch Mission");
    },
  },
};
</script>

<style scoped>
.chosen-heroes {
  display: flex;
  flex-flow: column;
  align-items: center;
}
.selectionContainer {
  display: flex;
  justify-content: center;
  align-content: center;
}
.btnContainer {
  display: flex;
}
.launchBtn {
  height: 50px;
  background: green;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 150px;
  border-radius: 5px;
  color: white;
  cursor: pointer;
  border: none;
}
</style>


