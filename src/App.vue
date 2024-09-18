<script setup>
  import { ref, onBeforeMount, computed, watch } from 'vue';

  import CoffeeCard from './components/CoffeeCard.vue';

  let coffeeList = ref([]);
  let filter = ref("");
  let filteredCoffees = ref([]);

  watch(filter, (newVal) => {
    if('all' === newVal){
      filteredCoffees.value = [...coffeeList.value];
      return;
    }

    filteredCoffees.value = coffeeList.value.filter(coffee => coffee.available);
  });

  onBeforeMount(async () => {
    coffeeList.value = await fetchItems();
    filter.value = 'all';
  });

  const fetchItems = async function(){
    let data = [];

    try{
      const response = await fetch('https://raw.githubusercontent.com/devchallenges-io/web-project-ideas/main/front-end-projects/data/simple-coffee-listing-data.json');

      if(!response.ok){
        alert('An unexpected error, has occured, try again later.')
        return;
      }

      data = await response.json();

    }catch(e){
      console.log(e)
      console.log({e})
    }

    return data;
  }
</script>

<template>
  <main>
    
    <header>
      <h1>Our Collection</h1>
  
      <p>
        Introducing our Coffee Collection, a selection of unique coffess from different roast types 
        and origins, expertly roasted in small batches and shipped fresh weekly.
      </p>
  
      <section>
        <button @click="filter = 'all'" :class="{ 'active': filter === 'all' }">All Products</button>
        <button @click="filter = 'available'" :class="{ 'active': filter === 'available' }">Available Now</button>
      </section>
    </header>
    
    <section class="coffee-grid">
      <CoffeeCard
        v-for="coffee in filteredCoffees"
        :key="coffee.id"
        :coffee="coffee"
      />
      
    </section>

  </main>
</template>

<style scoped>

</style>
