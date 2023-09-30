<script lang="ts">
import Game from './components/Game.vue';


export default {
    data() {
        return {
            logged: false
        };
    },
    components: { Game }
}
</script>

<template>
  <main>
    <Game></Game>
  </main>
</template>

<style>

</style>
