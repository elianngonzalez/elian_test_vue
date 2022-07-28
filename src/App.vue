<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import axios, { Axios } from 'axios';
import Categories from './components/Categories.vue';
import Meals from './components/Meals.vue'; 
import swal from 'sweetalert';
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="https://themealdb.com/images/logo-small.png" />

    <div class="wrapper">
      <HelloWorld msg="api de comida!" />

      <h3>deseas buscar una receta?</h3>
      <input type="text" v-model="search" v-on:keyup.enter="searchData" placeholder="buscar receta">

      <nav>
        <RouterLink to="/">Home</RouterLink>
  
        <RouterLink to="/about">About</RouterLink>
      </nav>
  
      <p>elian gonzalez</p>
  
      <hr />

      <RouterView />
    
    </div>
  </header>

    <Meals 
      v-for="meal in meals" 
      v-bind:key="meal.idMeal" 
      v-bind:meal="meal" 
      />

  <div class="text-center">....</div>
  <h3>O busca por categoria</h3>
    
    <Categories 
      v-for="category in paginated" 
      v-bind:key="category.idCategory" 
      v-bind:category="category" 
      /> 
<div class="text-center">
  <a @click="prev()">Anterior</a>
  |
  <a @click="next()">Siguiente</a>
  Actual : {{current}}
</div>

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
      current: 1,
      pageSize: 5,
    };
  },

  computed: {
      indexStart(){
        return (this.current - 1) * this.pageSize;
      },

      indexEnd(){
        return this.indexStart + this.pageSize;
      },

      paginated(){
        return this.categories.slice(this.indexStart, this.indexEnd);
      }
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
        swal('debes ingresar algo','','error');
      }
    },
    prev(){
      this.current--;
    },
    next(){
      this.current++;
      }
  },
}
</script>

<style scoped>

*{
  font-family: monospace;
}
.category_container {
  text-align: center;
  margin: 2rem;
  padding: 20px;
  border: 1px solid grey;
}


header {
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

</style>
