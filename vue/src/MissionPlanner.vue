<template>
  <div>
    <!-- justice leage application begins here -->
    <h1 id="jl">Justice League Mission Planner</h1>

    <ul class="roster">
      <h3>Roster:</h3>
      <li v-for="hero in heroes" :key="hero.name">
        <!-- to do: conditionally display this span -->
        <div v-for="selectedHero in chosenHeroesSelected" :key="selectedHero">
          <span v-if="selectedHero === hero.name">✔ &nbsp;</span>
        </div>

        <span>{{ hero.name }}&nbsp;</span>
        <span class="edit" @click="editHero(hero)">edit</span>
      </li>
      <br />
      <input
        type="text"
        placeholder="new name"
        v-model="newName"
        v-if="isEdit"
        @keyup.enter="changeName"
        @blur="clear"
      />
      <br />
      <span v-if="isEdit"
        >enter to submit, click outside the box to cancel</span
      >
    </ul>
    <ChosenHeroes
      :heroes="heroes"
      :chosenHeroesSelected="chosenHeroesSelected"
      @update:heroAdded="heroUpdated"
      @update:heroRemoved="heroRemovedFromSelected"
    />
  </div>
</template>

<script>
import ChosenHeroes from "./components/ChosenHeroes.vue";

export default {
  components: {
    ChosenHeroes,
  },
  data() {
    return {
      heroes: [
        { name: "Superman" },
        { name: "Batman" },
        { name: "Aquaman" },
        { name: "Wonder Woman" },
        { name: "Green Lantern" },
        { name: "Martian Manhunter" },
        { name: "Flash" },
      ],
      newName: "",
      isEdit: false,
      heroToModify: null,
      chosenHeroesSelected: [],
    };
  },
  methods: {
    editHero(hero) {
      this.isEdit = true;
      this.newName = hero.name;
      this.heroToModify = hero;
    },

    changeName() {
      this.heroToModify.name = this.newName;
      this.isEdit = false;
    },

    heroUpdated(name) {
      console.log(name);
      this.chosenHeroesSelected.push(name);
      // this.heroes = this.heroes.filter(h => h.name != name)
    },
    heroRemovedFromSelected(hero) {
      console.log(hero.name);
      this.chosenHeroesSelected = this.chosenHeroesSelected.filter(
        (h) => h != hero.name
      );
    },

    clear() {
      this.heroToModify = null;
      this.newName = "";
      this.isEdit = false;
    },
  },
};
</script>

<style scoped>
ul.roster {
  text-align: left;
}
li {
  display: flex;
  align-items: center;
}
.edit {
  color: blue;
  text-decoration: underline;
}
.edit:hover {
  cursor: pointer;
}
</style>
