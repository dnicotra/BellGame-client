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
        roomid: room.id,
        measurement: ""
        
    }    
    },
    async created() {
        room.onMessage("input", (message)=>{
            this.input = Boolean(message) ? "1" : "0" 
            this.btn0_enabled = true
            this.btn1_enabled = true 
            this.measurement = ""
             
        }
        )
        room.onMessage("outcome", (message=>{
            this.outcomes.push(message)
            if(Boolean(message)){
                this.n_won++
            }
            this.winningFrequency = this.n_won/this.outcomes.length
        }))

        room.onMessage("measurement",(message) => {
            this.measurement = String(message)
        })
    },

    methods: {
        answer0(){
            room.send("answer", false)
            this.btn1_enabled = false
        },
        answer1(){
            room.send("answer", true)
            this.btn0_enabled = false
        },
        measA0(){
            room.send("measureA0", "")
        },
        measA1(){
            room.send("measureA1", "")
        },
        measB0(){
            room.send("measureB0", "")
        },
        measB1(){
            room.send("measureB1", "")
        },
    }
}
</script>

<template>
    <main>
        <div id="charlie">
            <h1 class="title">Welcome to the Bell Game</h1>
            <p>Session: {{ sessionid }} - Room: {{ roomid }} </p>
            <h1 class="section">Charlie says</h1>
            <h1 class="charlie" v-show="input!='?'" >{{ input }}</h1>
            <div class="btn_grid">
                <button class="btn" :disabled="!btn0_enabled" @click="answer0">Answer 0</button>
                <button class="btn" :disabled="!btn1_enabled" @click="answer1">Answer 1</button>
            </div>
            
            <p v-if="outcomes.length > 0"> Winning frequency: {{ (100 * winningFrequency).toFixed(2) }} <span>&#177;</span> {{ (100*3*Math.sqrt((1-winningFrequency)*winningFrequency/outcomes.length)).toFixed(2) }} %</p>
            <p class="n_won" v-if="outcomes.length > 0"> You won {{ n_won }} times</p>
            <p class="n_lost" v-if="outcomes.length > 0"> You lost {{ outcomes.length - n_won }} times</p>
           
        </div>
        <div id="qubit">
            <h1 class="section">Entangled Qubit</h1>
            <p>Choose your measurement basis</p>
            <div class="meas_btn_grid">
                <button  class="btnA"  @click="measA0">A0</button>
                <button  class="btnA"  @click="measA1">A1</button>
                <button  class="btnB"  @click="measB0">B0</button>
                <button  class="btnB"  @click="measB1">B1</button>
            </div>
            <h2 v-if="measurement.length>0">Measurement outcome</h2>
            <h1 class="charlie" v-if="measurement.length>0">{{ measurement }}</h1>
        </div>
        
    </main>
</template>

<style>

#charlie{
    padding-bottom: 10rem;
}
.charlie{
    font-size: xxx-large;
    text-align: center;
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

.meas_btn_grid{
    display: grid;
    grid-template-columns: auto auto;
    column-gap: 1rem;
    row-gap: 1rem;
}

.btn{
    font-size: xx-large;
    font-weight: bold;
}

.btnA{
    font-size: xx-large;
    font-weight: bold;
    color: skyblue;
}

.btnB{
    font-size: xx-large;
    font-weight: bold;
    color:coral;
}
</style>