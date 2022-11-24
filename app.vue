<template>
  <div class="container">
    <!-- <NuxtWelcome /> -->
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click the "Find Names" button bellow</p>
    <div class="options-container">
      <div class="option-container">
        <h4>1) Choose a gender</h4>
        <div class="option-buttons">
          <button
            class="option option-left"
            :class="options.gender == Gender.BOY && 'option-active'"
            @click="options.gender = Gender.BOY"
          >
            Boy
          </button>
          <button
            class="option"
            :class="options.gender == Gender.UNISEX && 'option-active'"
            @click="options.gender = Gender.UNISEX"
          >
            Unisex
          </button>
          <button
            class="option option-right"
            :class="options.gender == Gender.GIRL && 'option-active'"
            @click="options.gender = Gender.GIRL"
          >
            Girl
          </button>
        </div>
      </div>
      <div class="option-container">
        <h4>2) Choose the name's popularity</h4>
        <div class="option-buttons">
          <button
            class="option option-left"
            :class="options.popularity == Popularity.TRENDY && 'option-active'"
            @click="options.popularity = Popularity.TRENDY"
          >
            Trendy
          </button>
          <button
            class="option option-right"
            :class="options.popularity == Popularity.UNIQUE && 'option-active'"
            @click="options.popularity = Popularity.UNIQUE"
          >
            Unique
          </button>
        </div>
      </div>
      <div class="option-container">
        <h4>3) Choose name's length</h4>
        <div class="option-buttons">
          <button
            class="option option-left"
            :class="options.length == Length.LONG && 'option-active'"
            @click="options.length = Length.LONG"
          >
            Long
          </button>
          <button
            class="option"
            :class="options.length == Length.ALL && 'option-active'"
            @click="options.length = Length.ALL"
          >
            All
          </button>
          <button
            class="option option-right"
            :class="options.length == Length.SHORT && 'option-active'"
            @click="options.length = Length.SHORT"
          >
            Short
          </button>
        </div>
      </div>
      <button class="primary" @click="computeSelectedNames">Find Names</button>
    </div>
    <div class="card-container">
      <div class="card" v-for="(name, index) in selectedNames" :key="name">
        <h4>{{ name }}</h4>
        <p @click="deleteName(index)">
          x
        </p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Gender, Popularity, Length, names } from './data'

interface OptionState{
  gender: Gender;
  popularity: Popularity;
  length: Length;
}

const options = reactive<OptionState>({
  gender: Gender.GIRL,
  popularity: Popularity.TRENDY,
  length: Length.ALL,
})

const selectedNames = ref<string[]>([])

const computeSelectedNames = () => {
  const filteredNames = names
  .filter(e => e.gender === options.gender)
  .filter(e => e.popularity === options.popularity)
  .filter(e => {
    if(options.length === Length.ALL){
      return true
    }else{
      return e.length === options.length
    }
  })

  selectedNames.value = filteredNames.map(e => e.name)
}

const deleteName = (index) => {
  selectedNames.value.splice(index, 1)
}
</script>

<style scoped>
.container {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(27, 60, 138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}

.container h1 {
  font-size: 3rem;
}

.options-container {
  background-color: rgb(255, 238, 236);
  border-radius: 2rem;
  padding: .1rem;
  width: 95%;
  margin: 0 auto;
  margin-top: 4rem;
  position: relative;
}

.option-container {
  margin-bottom: 2rem;
}

.option {
  background-color: white;
  outline: 0.15rem solid rgb(249, 87, 89);
  border: none;
  padding: 0.75rem;
  width: 12rem;
  font-size: 1rem;
  color: rgb(27, 60, 138);
  cursor: pointer;
  font-weight: 200;
}

.option-left {
  border-radius: 1rem 0 0 1rem;
}

.option-right {
  border-radius: 0 1rem 1rem 0;
}

.option-active {
  background-color: rgb(249, 87, 89);
  color: white;
}

.primary{
  background-color: rgb(249, 87, 89);
  color: white;
  border-radius: 6.5rem;
  border: none;
  padding: .75rem 4rem;
  font-size: 1rem;
  margin-top: 1rem;
  cursor: pointer;
}

.card-container{
  display: flex;
  margin-top: 3rem;
  flex-wrap: wrap;
}

.card{
  background-color: rgb(27, 60, 138);
  width: 28%;
  color: white;
  border-radius: 1rem;
  padding: 1rem;
  margin-right: .5rem;
  margin-bottom: 1rem;
  position: relative;
}

.card p{
  position: absolute;
  top: -15%;
  left: 93%;
  cursor: pointer;
  color: rgba(255, 255, 255, .178);
}
</style>
