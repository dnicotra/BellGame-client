<script lang="ts">
import * as Colyseus from "colyseus.js"
const client = new Colyseus.Client("wss://bell-game-server.app.cern.ch")
let room = await client.joinOrCreate("bell_game_room")

export default {
    data() {
    return {
        input: "?",
        outcomes: new Array,
        winningFrequency: 0,
        n_won : 0,
        btn0_enabled : false,
        btn1_enabled : false,
        sessionid : room.sessionId,
        roomid: room.id
        
    }    
    },
    created() {
        room.onMessage("input", (message)=>{
            this.input = Boolean(message) ? "1" : "0" 
            this.btn0_enabled = true
            this.btn1_enabled = true 
        }
        )
        room.onMessage("outcome", (message=>{
            this.outcomes.push(message)
            if(Boolean(message)){
                this.n_won++
            }
            this.winningFrequency = this.n_won/this.outcomes.length
        }))
    },

    methods: {
        answer0(){
            room.send("answer", "false")
            this.btn1_enabled = false
        },
        answer1(){
            room.send("answer", "true")
            this.btn0_enabled = false
        },
    }
}
</script>

<template>
    <main>
        <div id="charlie">
            <h1 class="title">Welcome to the Bell Game</h1>
            <p>Session: {{ sessionid }} - Room: {{ roomid }} </p>
            <h1 class="section">Charlie</h1>
            <h1 class="charlie" v-show="input!='?'" >Charlie says {{ input }}</h1>
            <div class="btn_grid">
                <button class="btn" :disabled="!btn0_enabled" @click="answer0">Answer 0</button>
                <button class="btn" :disabled="!btn1_enabled" @click="answer1">Answer 1</button>
            </div>
            
            <p v-if="outcomes.length > 0"> Winning frequency: {{ (100 * winningFrequency).toFixed(2) }}%</p>
            <p class="n_won" v-if="outcomes.length > 0"> You won {{ n_won }} times</p>
            <p class="n_lost" v-if="outcomes.length > 0"> You lost {{ outcomes.length - n_won }} times</p>
           
        </div>
        <div id="qubit">
            <h1 class="section">Your qubit</h1>

        </div>
        
    </main>
</template>

<style>

#charlie{
    padding-bottom: 10rem;
}

.section{

    font-weight: bold;
    font-size:x-large;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;

    color: rgb(218, 218, 218);
}
.title{
    color: orange;
    font-weight: bold;
    font-size:xx-large;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
}
.n_won{
    color: green;
}
.n_lost{
    color: red;
}

.btn_grid{
    display: grid;
    grid-template-columns: auto auto;
    column-gap: 3rem;
}

.btn{
    font-size: larger;
    font-weight: bold;
}
</style>