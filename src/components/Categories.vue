<template>
    <div class="category_container" >
        <h3>{{category.strCategory}}</h3>
        <img v-bind:src="category.strCategoryThumb" alt="">
        <p>{{category.strCategoryDescription}}</p>
    </div>
</template>

<script>
export default {
    props: ['category']
}
</script>