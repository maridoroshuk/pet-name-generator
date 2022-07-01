<script setup lang="ts">
import { Gender, Popularity, Length, names } from "@/data"

interface OptionsState {
  gender: Gender;
  popularity: Popularity;
  length: Length
}

const options = reactive<OptionsState>({
  gender: Gender.GIRL,
  popularity: Popularity.UNIQUE,
  length: Length.ALL
})

const computeSelectedNames = () => {
  const filteredNames = names
    .filter((name) => name.gender === options.gender)
    .filter((name) => name.popularity === options.popularity)
    .filter((name) => {
      if (options.length === Length.ALL) return true
      else return name.length === options.length
    })

  selectedNames.value = filteredNames.map(name => name.name)
}

const selectedNames = ref<string[]>([])

const removeName = (index: number) => {
  const filteredNames = [...selectedNames.value]
  filteredNames.splice(index, 1)
  selectedNames.value = filteredNames
}

const optionsArray = [
  {
    title: "1) Choose a gender",
    category: "gender",
    buttons: [Gender.GIRL, Gender.BOY, Gender.UNISEX]
  },
  {
    title: "2) Choose the name's popularity",
    category: "popularity",
    buttons: [Popularity.TRENDY, Popularity.UNIQUE]
  },
  {
    title: "3) Choose the name's length",
    category: "length",
    buttons: [Length.ALL, Length.LONG, Length.SHORT]
  }
]

</script>

<template>
  <div class="container">
    <h1>Pet Name Generator</h1>
    <p>Choose your options and click the "Find Names" buttom below</p>
    <div class="options-container">
      <Option v-for="option in optionsArray" :key="option.title" :option="option" :options="options" />
      <button class="primary" @click="computeSelectedNames">Find Names</button>
    </div>
    <div class="cards-container">
      <CardName v-for="(name, index) in selectedNames" :key="name" :name="name" :index="index"
        @remove="() => removeName(index)" />
    </div>
  </div>
</template>

<style scoped>
.container {
  font-family: Arial, Helvetica, sans-serif;
  color: #274954;
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}

.container h1 {
  font-size: 3rem;
}

.primary {
  background-color: #70A9A1;
  color: white;
  border-radius: 6.5rem;
  border: none;
  padding: 0.75rem 4rem;
  font-size: 1rem;
  margin-top: 1rem;
  cursor: pointer;
}

.options-container {
  background-color: #F6F1D1;
  border-radius: 2rem;
  padding: 1rem;
  width: 95%;
  margin: 0 auto;
  margin-top: 4rem;
  position: relative;
}

.cards-container {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 3rem;
  flex-wrap: wrap;
}
</style>


