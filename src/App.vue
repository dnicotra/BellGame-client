<script lang="ts">
import Game from './components/Game.vue';


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
    components: { Game }
}
</script>

<template>
  <main>
    <Game einstein_room="bell_game_room" v-if="!einstein()"></Game>
    <Game einstein_room="bell_game_room_einstein" v-if="einstein()"></Game>
  </main>
</template>

<style>

</style>
