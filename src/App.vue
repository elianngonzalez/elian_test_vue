<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import axios, { Axios } from 'axios';
import Categories from './components/Categories.vue';
import Meals from './components/Meals.vue'; 
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="https://themealdb.com/images/logo-small.png" />

    <div class="wrapper">
      <HelloWorld msg="api de comida!" />

      <h2>deseas buscar una receta?</h2>
      <input type="text" v-model="search" v-on:keyup.enter="searchData" placeholder="buscar receta">

      <nav>
        <RouterLink to="/">Home</RouterLink>
  
        <RouterLink to="/about">About</RouterLink>
      </nav>
  
      <p>elian gonzalez</p>
  
      <hr />

    
    <Meals 
      v-for="meal in meals" 
      v-bind:key="meal.idMeal" 
      v-bind:meal="meal" 
      />



        <RouterView />

      <Categories 
      v-for="category in categories" 
      v-bind:key="category.idCategory" 
      v-bind:category="category" 
      />

    </div>
  </header>


</template>
<script>
export default {
  name: 'App',
  components: { 
    Categories,
    Meals,
    },
  data() {
    return {
      categories: [],
      meals: [],
      search: null,
    };
  },

  mounted() {
    axios.get('https://themealdb.com/api/json/v1/1/categories.php')
      .then((res) => {
        console.log(res.data.categories)
        //idCategory - strCategory - strCategoryDescription - strCategoryThumb
        this.categories = res.data.categories
      })
      .catch((err) => {
        console.log(err)
      })
  },

  methods: {
    searchData(){
      //verifica si el campo de busqueda tiene texto
      if (this.search){
        axios.get('https://themealdb.com/api/json/v1/1/search.php?s=' + this.search)
        .then(res => {
          this.meals =res.data.meals;
        })
        .catch((err) => {
          console.log(err)
        });
      }else{
        alert('deves ingresar algo');
      }
    }
  },
}
</script>

<style scoped>
.category_container {
  display: block;
  align-items: center;
  text-align: center;
  margin-top: 2rem;
  border: 1px solid grey;
}


header {
  text-align: center;
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
