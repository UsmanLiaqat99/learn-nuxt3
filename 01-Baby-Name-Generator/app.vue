<template>
  <div class="container">
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click the "Find Names" bottom below</p>

    <div class="options-container">
      <Option
        v-for="option in optionsArray"
        :key="option"
        :option="option"
        :options="options"
      />

      <button class="primary" @click="computeSelectedNames">Find Names</button>
    </div>
    <div class="cards-container">
      <div v-for="name in selectedNames" :key="name" class="card">
        <h4>{{ name }}</h4>
        <p>x</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Gender, Popularity, Length, names } from "./data";

interface OptionsState {
  gender: Gender;
  popularity: Popularity;
  length: Length;
}

const options = reactive<OptionsState>({
  gender: Gender.GIRL,
  length: Length.ALL,
  popularity: Popularity.UNIQUE,
});

const computeSelectedNames = () => {
  const filterNames = names
    .filter((name) => name.gender === options.gender)
    .filter((name) => name.popularity === options.popularity)
    .filter((name) => {
      if (options.length === Length.ALL) return true;
      else return name.length === options.length;
    });

  selectedNames.value = filterNames.map((name) => name.name);
};

const selectedNames = ref<string[]>([]);

const optionsArray = [
  {
    category: "gender",
    title: "1) Choose a gender",
    buttons: [Gender.BOY, Gender.GIRL, Gender.UNISEX],
  },
  {
    category: "popularity",
    title: "2) Choose the name's popularity",
    buttons: [Popularity.TRENDY, Popularity.UNIQUE],
  },
  {
    category: "length",
    title: "3) Choose the name's length",
    buttons: [Length.LONG, Length.ALL, Length.SHORT],
  },
];
</script>

<style scoped>
.container {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(27, 60, 138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}
h1 {
  font-size: 3rem;
}
.options-container {
  background-color: rgb(255, 238, 236);
  border-radius: 2rem;
  padding: 1rem;
  width: 95%;
  margin: 0 auto;
  margin-top: 4rem;
  position: relative;
}
.primary {
  background: rgb(249, 87, 89);
  color: white;
  border: none;
  padding: 0.85rem 4rem;
  font-size: 1rem;
  border-radius: 6.5rem;
  cursor: pointer;
  margin-top: 1rem;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 2rem;
}

.card {
  background-color: rgb(27, 60, 138);
  width: 28%;
  color: white;
  border-radius: 1rem;
  padding: 0.1rem;
  margin-right: 0.5rem;
  margin-bottom: 1rem;
}

.card p {
  position: absolute;
  top: -30%;
  left: 92.5%;
  cursor: pointer;
  color: rgb(225, 225, 225, 0.178);
}
</style>
