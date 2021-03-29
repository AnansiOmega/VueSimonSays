<template>
  <div class="block-container">
    <h1 v-if="gameOver && notes.length >= 1 ">You lost! You were able to get {{notes.length - 1}} notes. Good Job.</h1>
    <p v-if="gameOver && notes.length >= 1 " class='hint'>(you can just hit space again to restart)</p>
    <svg id='simon' width="80vw" height="80vh" viewBox="0 0 744 700" fill="none" xmlns="http://www.w3.org/2000/svg">
    <mask id="path-1-inside-1" fill="white">
    <path d="M557.907 163.969C582.329 188.391 601.701 217.384 614.918 249.292C628.135 281.201 634.938 315.401 634.938 349.938C634.938 384.476 628.135 418.675 614.918 450.584C601.701 482.493 582.329 511.485 557.907 535.907L371.938 349.938L557.907 163.969Z"/>
    </mask>
    <!-- Red block -->
    <path id='block-E' @click='playNote("w", true)' d="M557.907 163.969C533.485 139.547 504.492 120.175 472.584 106.958C440.675 93.7409 406.476 86.9382 371.938 86.9382C337.4 86.9382 303.201 93.7409 271.292 106.958C239.384 120.175 210.391 139.547 185.969 163.969L371.938 349.938L557.907 163.969Z" stroke="black" stroke-width="6" mask="url(#path-3-inside-3)"/>
    <mask id="path-4-inside-4" fill="white">
    <path d="M185.969 535.907C210.391 560.329 239.384 579.701 271.292 592.918C303.201 606.135 337.4 612.938 371.938 612.938C406.476 612.938 440.675 606.135 472.584 592.918C504.492 579.701 533.485 560.329 557.907 535.907L371.938 349.938L185.969 535.907Z"/>
    </mask>
    <!-- Blue Block -->
    <path id='block-A' @click='playNote("a", true)' d="M185.969 163.969C161.547 188.391 142.175 217.384 128.958 249.292C115.741 281.201 108.938 315.401 108.938 349.938C108.938 384.476 115.741 418.675 128.958 450.584C142.175 482.493 161.547 511.485 185.969 535.907L371.938 349.938L185.969 163.969Z"  stroke="black" stroke-width="6" mask="url(#path-2-inside-2)"/>
    <mask id="path-3-inside-3" fill="white">
    <path d="M557.907 163.969C533.485 139.547 504.492 120.175 472.584 106.958C440.675 93.7409 406.476 86.9382 371.938 86.9382C337.4 86.9382 303.201 93.7409 271.292 106.958C239.384 120.175 210.391 139.547 185.969 163.969L371.938 349.938L557.907 163.969Z"/>
    </mask>
    <!-- Yelow Block -->
    <path id='block-D' @click='playNote("d", true)' d="M557.907 163.969C582.329 188.391 601.701 217.384 614.918 249.292C628.135 281.201 634.938 315.401 634.938 349.938C634.938 384.476 628.135 418.675 614.918 450.584C601.701 482.493 582.329 511.485 557.907 535.907L371.938 349.938L557.907 163.969Z"  stroke="black" stroke-width="6" mask="url(#path-1-inside-1)"/>
    <mask id="path-2-inside-2" fill="white">
    <path d="M185.969 163.969C161.547 188.391 142.175 217.384 128.958 249.292C115.741 281.201 108.938 315.401 108.938 349.938C108.938 384.476 115.741 418.675 128.958 450.584C142.175 482.493 161.547 511.485 185.969 535.907L371.938 349.938L185.969 163.969Z"/>
    </mask>
    <!-- Blue Block -->
    <path id='block-G' @click='playNote("s", true)' d="M185.969 535.907C210.391 560.329 239.384 579.701 271.292 592.918C303.201 606.135 337.4 612.938 371.938 612.938C406.476 612.938 440.675 606.135 472.584 592.918C504.492 579.701 533.485 560.329 557.907 535.907L371.938 349.938L185.969 535.907Z"  stroke="black" stroke-width="6" mask="url(#path-4-inside-4)"/>
    <circle id='start-game-btn' @click='startGame' cx="372" cy="350" r="97"/>
    <text id='button-text' x="340" y="360">Start</text>
    </svg>
    <h1 class='title'>A simple Simon Says game built from scratch with Vue 3</h1>
    <p class='hint'>(You can use w a s d to press the blocks. Also, spacebar will start the game)</p>

  </div>
  <!-- why do I have to display state in some way for onUpdated to work? -->
  <ul :key="note" v-for="note in userNotes">
      <!-- <li>{{note}}</li> -->
  </ul>
</template>



<script>
import { defineComponent, onMounted, reactive, toRefs, onUpdated } from "vue";

import Ekey from '../MusicNotes/piano-e_E_major.wav'
import Akey from '../MusicNotes/piano-a_A_major.wav'
import Dkey from '../MusicNotes/piano-d_D_major.wav'
import Gkey from '../MusicNotes/piano-g_G_major.wav'

export default defineComponent({
  setup: () => {
    const state = reactive({
      availableNotes: ['w','a','d','s'],
      notes: [],
      userNotes: [],
      index: 0,
      gameOver: true,
      color: '#f0ead6'
    });

    onMounted(() => {
        window.addEventListener("keyup", e => {
            // this is neccessary because if the user hits anything other than the available notes
            // then it gets pushed into userNotes array and breaks the logic
            if(e.code === 'Space') startGame()
            if(e.key === state.availableNotes.find(letter => letter === e.key)) playNote(e.key, true)
        })
    })

    const playNote = (key, user) => {
        if(user) state.userNotes.push(key)
        let audio;
        let note;
        switch(key){
            case "w":
              //red
            audio = new Audio(Ekey);
            note = "E"
            state.color = '#E91F0C'
            break;
            case "a":
              //blue
            audio = new Audio(Akey);
            note = "A"
            state.color = '#1A81DB'
            break;
            case "d":
              //yellow
            audio = new Audio(Dkey);
            note = "D"
            state.color = '#FCF300'
            break;
            case "s":
              //green
            audio = new Audio(Gkey);
            note = "G"
            state.color = '#53CA2B'
            break;
            default:
            return
        }

        // for the animations to work with keypres, I need to change the name of the svg's so that the animations reset
        // I'm sure there's a better way to do this but I wasn't able to figure it out... but this works sooo.

        let block = document.getElementById(`block-${note}`) || document.getElementById(`block-${note}-active`)
        block.id === `block-${note}` ? block.id = `block-${note}-active` : block.id = `block-${note}`

        let button = document.getElementById('start-game-btn') || document.getElementById('start-game-btn-active')
        button.id === 'start-game-btn' ? button.id = 'start-game-btn-active' : button.id = 'start-game-btn'

        let simon = document.getElementById('simon') || document.getElementById('simon-active')
        simon.id === 'simon' ? simon.id = 'simon-active' : simon.id = 'simon'

        audio.volume = 0.1
        audio.play()
    }

    const startGame = () => {
      state.gameOver = false
      state.notes = []
      playGame()
    }
      //starts the game and also acts as the 'bot' for the game
    const playGame = () => {
        createNewNote()
        state.userNotes = []
        state.index = 0
        let i = 0
        /// makes a random note, adds it to state and plays them at an interval
        // kinda like a slow for loop is how this is being treated
        const id = setInterval(() => {
            playNote(state.notes[i])
            i++
            if(!state.notes[i]) clearInterval(id)
        }, 500)
    }

/// takes a random 'note' from states array. then adds is to the 'bots' list of notes
    const createNewNote = () => {
        let index = Math.floor(Math.random() * state.availableNotes.length)
        state.notes.push(state.availableNotes[index])
    };

/// checks on the state, and the users inputs and makes sure that the arrays are equal, if not then game over.
// if user is correct, it runs 'playgame' which will add a new note, and then play through all the notes all over again.
    onUpdated(() => {
        if(state.gameOver) return
        if(!state.userNotes[state.index]) return
        if(state.userNotes[state.index] === state.notes[state.index]){
            state.index ++
        } else {
            state.gameOver = true
        }
        if(state.index === state.notes.length){
            playGame()
        }
    })


    return {
        ...toRefs(state),
        playNote,
        startGame
    }
  },
});
</script>


<style>
body{ 
  background-color: #9D79BC;
}

#simon {
  animation: simon-pulse 1s forwards;
}
#simon-active {
  animation: simon-pulse-active 1s forwards;
}

@keyframes simon-pulse {
    from{
    -webkit-filter: drop-shadow( 0px 0px 5px v-bind(color));
      filter: drop-shadow( 0px 0px 5px v-bind(color));
    }
    to{
    -webkit-filter: drop-shadow( 0px 0px 0px rgba(0, 0, 0, 0));
    filter: drop-shadow( 0px 0px 0px rgba(0, 0, 0, 0));
    }
}

@keyframes simon-pulse-active {
    from{
    -webkit-filter: drop-shadow( 0px 0px 5px v-bind(color));
    filter: drop-shadow( 0px 0px 5px v-bind(color));
    }
    to{
    -webkit-filter: drop-shadow( 0px 0px 0px rgba(0, 0, 0, 0));
    filter: drop-shadow( 0px 0px 0px rgba(0, 0, 0, 0));
    }
}

#block-E {
  animation: red-pulse-block 1s forwards;
  fill: #E91F0C;
  cursor: pointer;
}

#block-E-active {
  animation: red-pulse-active 1s forwards;
  fill: #F9938A;
  cursor: pointer;
}
/* I hate this so much pls help */
@keyframes red-pulse-block {
    from{fill: #F9938A;}
    to{fill: #E91F0C;}
}

@keyframes red-pulse-active {
    from{fill: #F9938A;}
    to{fill: #E91F0C;}
}

#block-A {
  animation: blue-pulse-block 1s forwards;
  fill: #1A81DB;
  cursor: pointer;
}

#block-A-active {
  animation: blue-pulse-active 1s forwards;
  fill: #91C4F2;
  cursor: pointer;
}
/* I hate this so much pls help */
@keyframes blue-pulse-block {
    from{fill: #91C4F2;}
    to{fill: #1A81DB;}
}

@keyframes blue-pulse-active {
    from{fill: #91C4F2;}
    to{fill: #1A81DB;}
    
}
/* down 10 places ?  */
#block-D {
  animation: yellow-pulse-block 1s forwards;
  fill: #FCF300;
  cursor: pointer;
}
#block-D-active {
  animation: yellow-pulse-active 1s forwards;
  fill: #FFFC99;
  cursor: pointer;
}

@keyframes yellow-pulse-block {
    from{fill: #FFFC99;}
    to{fill: #FCF300;}
}

@keyframes yellow-pulse-active {
    from{fill: #FFFC99;}
    to{fill: #FCF300;}
}

#block-G {
  animation: green-pulse-block 1s forwards;
  fill: #53CA2B;
  cursor: pointer;
}
#block-G-active {
  animation: green-pulse-active 1s forwards;
  fill: #BBEEAA;
  cursor: pointer;
}

@keyframes green-pulse-block {
    from{fill: #BBEEAA;}
    to{fill: #53CA2B;}
}

@keyframes green-pulse-active {
    from{fill: #BBEEAA;}
    to{fill: #53CA2B;}
}

#start-game-btn {
  animation: color-pulse 1s forwards;
  fill: #f0ead6;
  stroke: black;
  stroke-width: .4rem;
  cursor: pointer;
}

#start-game-btn-active {
  animation: color-pulse-active 1s forwards;
  fill: #f0ead6;
  stroke: black;
  stroke-width: .4rem;
  cursor: pointer;
}

@keyframes color-pulse {
  from{fill: v-bind(color)}
  to{fill: #f0ead6}
}
@keyframes color-pulse-active {
  from{fill: v-bind(color)}
  to{fill: #f0ead6}
}
#button-text{
  font-family: Avenir, Helvetica, Arial, sans-serif;
  fill: #2c3e50;
  font-size: 2rem;
  pointer-events: none;
}

.title {
  margin-top: 1%;
  margin-bottom: 1%;
}

.hint { 
  margin-top: 1%;
  margin-bottom: 1%;
}
</style>

