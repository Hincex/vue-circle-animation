<template>
    <div id="app">
        <circle-ani />
    </div>
</template>

<script>
import CircleAni from './components/Circle.vue';

export default {
    name: 'app',
    components: {
        CircleAni,
    },
};
</script>

<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
