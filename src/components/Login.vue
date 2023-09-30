<script lang="ts">
export default {
    data(){
        return {
            username: ""
            }
    }
}


</script>

<template>
<h1>Welcome to the BellGame</h1>
<input v-model="username" placeholder="Enter your name">
<button>Login</button>
</template>

<style>

</style>