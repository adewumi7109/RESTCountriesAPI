<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>
<template>
    <nav>
        <h1>Where in the world?</h1>
        <span>Dark Mode</span>
    </nav>

</template>


<style scoped>
nav {
    display: flex;
    justify-content: space-between;
    height: 10vh;
    padding: 0 50px;
    align-items: center;
    box-shadow: 0 2px 4px 0 rgba(0, 0, 0, .2);
    background-color: white;
}
</style>
