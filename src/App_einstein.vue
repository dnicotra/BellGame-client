<script lang="ts">
import Game_einstein from './components/Game_einstein.vue';


export default {
    data() {
        return {
            logged: false
        };
    },
    components: { Game_einstein }
}
</script>

<template>
  <main>
    <Game_einstein></Game_einstein>
  </main>
</template>

<style>

</style>
