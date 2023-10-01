<script lang="ts">
import Game from './components/Game.vue';
import Game_einstein from './components/Game_einstein.vue';


export default {
    data() {
        return {
            logged: false
        };
    },
    methods:{
      einstein(){
        return new URLSearchParams(window.location.search).has('einstein')
      }
    },
    components: { Game, Game_einstein }
}
</script>

<template>
  <main>
    <Game v-if="!einstein()"></Game>
    <Game_einstein v-if="einstein()"></Game_einstein>
  </main>
</template>

<style>

</style>
