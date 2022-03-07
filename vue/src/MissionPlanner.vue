<template>
  <div>
    <!-- justice leage application begins here -->
    <h1 id="jl">Justice League Mission Planner</h1>

    <ul class="roster">
      <h3>Roster:</h3>
      <li v-for="hero in heroes" :key="hero.id">
        <!-- to do: conditionally display this span -->
        <div
          v-for="selectedHero in chosenHeroesSelected"
          :key="selectedHero.id"
        >
          <span v-if="selectedHero.name === hero.name">✔ &nbsp;</span>
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
        @keyup.enter="changeName(heroToModify)"
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
      :heroToModify="heroToModify"
      :newName="newName"
      @update:heroAdded="heroUpdated"
      @update:heroRemoved="heroRemovedFromSelected"
    />
  </div>
</template>

<script>
import { v4 as uuidv4 } from "uuid";
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
      trackModifiedHero: null,
    };
  },
  created() {
    this.heroes.map((hero) => {
      return (hero.id = uuidv4());
    });
    console.log(this.heroes, "generating uuids for data");
  },

  methods: {
    editHero(hero) {
      this.isEdit = true;
      this.newName = hero.name;
      this.heroToModify = hero;
      console.log(hero, "edit hero function");
    },

    changeName(hero) {
      this.trackModifiedHero = hero;
      if (
        hero.name.toLowerCase().localeCompare(this.newName.toLowerCase()) === 0
      ) {
        return alert("name already exists");
      }
      if (
        hero.name.toLowerCase().localeCompare(this.newName.toLowerCase()) ===
          -1 ||
        hero.name.toLowerCase().localeCompare(this.newName.toLowerCase()) === 1
      ) {
        this.heroToModify.name = this.newName;
        this.isEdit = false;
      }

      for (let i = 0; i < this.chosenHeroesSelected.length; i++) {
        console.log("changed in update", this.chosenHeroesSelected[i]);
        if (hero.id === this.chosenHeroesSelected[i].id) {
          this.chosenHeroesSelected[i].name = this.newName;
        }
      }
    },

    heroUpdated(data) {
      this.chosenHeroesSelected.push(data);
    },
    heroRemovedFromSelected(hero) {
      // console.log(hero, 'removed hero');
      this.chosenHeroesSelected = this.chosenHeroesSelected.filter(
        (h) => h.id !== hero.id
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
