<template>
<v-row class="centrado">
    <Nuxt />
</v-row>
</template>

<script>
export default {
    name: 'Login'
}
</script>

<style scoped>

.centrado {
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    background-color: aqua;
}
</style>
