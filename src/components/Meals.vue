<template>
    <div >
        <h3>{{ meal.strMeal }}</h3>
        <img v-bind:src="meal.strMealThumb" alt="" width="300">
        <p><br>Categoria<br>{{ meal.strCategoria }} | Origen: {{ meal.strArea }}</p>
    </div>
</template>

<script>
export default {
    props: ['meal']
}
</script>