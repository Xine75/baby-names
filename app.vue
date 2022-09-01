<script lang="ts" setup>
import {Gender, Length, names, Popularity} from "~/data";

interface OptionsState {
  gender: Gender;
  popularity: Popularity;
  length: Length;

}
const options = reactive<OptionsState>({
  gender: Gender.GIRL,
  popularity: Popularity.UNIQUE,
  length: Length.ALL,
});

const computeSelectedNames = () => {
  const filteredNames = names
      .filter((name) => name.gender === options.gender)
      .filter((name) => name.popularity === options.popularity)
      .filter((name) => {
        if(options.length === Length.ALL) return true
        else return name.length === options.length
      })
  selectedNames.value = filteredNames.map(name => name.name)
}

const selectedNames = ref<string[]>([])

const optionsArray = [
  {
    title: "1) Choose a gender",
    category: "gender",
    buttons: [Gender.GIRL, Gender.UNISEX, Gender.BOY]
  },
  {
    title: "2) Choose name popularity",
    category: "popularity",
    buttons: [Popularity.UNIQUE, Popularity.TRENDY]
  },
  {
    title: "3) Choose name length",
    category: "length",
    buttons: [Length.SHORT, Length.ALL, Length.LONG]
  },
]
</script>

<template>
  <div class="container">
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click the "Find Names" button below</p>
    <div class="options-container">
          <Option
              v-for="option in optionsArray"
              :key="option.title"
              :option="option"
              :options="options"
          />
      <button class="primary" @click="computeSelectedNames">Find Names</button>
    </div>
    <div class="cards-container">
      <div
          v-for="name in selectedNames"
          :key="name"
          class="card">
        {{ name }}
        <p>x</p>
      </div>
    </div>
  </div>
</template>

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

</style>
