<template>
  <div class="block-container">
    <svg width="744" height="700" viewBox="0 0 600 650" fill="none" xmlns="http://www.w3.org/2000/svg">
    <mask id="path-1-inside-1" fill="white">
    <path d="M557.907 163.969C582.329 188.391 601.701 217.384 614.918 249.292C628.135 281.201 634.938 315.401 634.938 349.938C634.938 384.476 628.135 418.675 614.918 450.584C601.701 482.493 582.329 511.485 557.907 535.907L371.938 349.938L557.907 163.969Z"/>
    </mask>
    <path id='block-C' @click='playNote("d", true)' d="M557.907 163.969C582.329 188.391 601.701 217.384 614.918 249.292C628.135 281.201 634.938 315.401 634.938 349.938C634.938 384.476 628.135 418.675 614.918 450.584C601.701 482.493 582.329 511.485 557.907 535.907L371.938 349.938L557.907 163.969Z"  stroke="black" stroke-width="6" mask="url(#path-1-inside-1)"/>
    <mask id="path-2-inside-2" fill="white">
    <path d="M185.969 163.969C161.547 188.391 142.175 217.384 128.958 249.292C115.741 281.201 108.938 315.401 108.938 349.938C108.938 384.476 115.741 418.675 128.958 450.584C142.175 482.493 161.547 511.485 185.969 535.907L371.938 349.938L185.969 163.969Z"/>
    </mask>
    <path id='block-B' @click='playNote("a", true)' d="M185.969 163.969C161.547 188.391 142.175 217.384 128.958 249.292C115.741 281.201 108.938 315.401 108.938 349.938C108.938 384.476 115.741 418.675 128.958 450.584C142.175 482.493 161.547 511.485 185.969 535.907L371.938 349.938L185.969 163.969Z"  stroke="black" stroke-width="6" mask="url(#path-2-inside-2)"/>
    <mask id="path-3-inside-3" fill="white">
    <path d="M557.907 163.969C533.485 139.547 504.492 120.175 472.584 106.958C440.675 93.7409 406.476 86.9382 371.938 86.9382C337.4 86.9382 303.201 93.7409 271.292 106.958C239.384 120.175 210.391 139.547 185.969 163.969L371.938 349.938L557.907 163.969Z"/>
    </mask>
    <path id='block-A' @click='playNote("w", true)' d="M557.907 163.969C533.485 139.547 504.492 120.175 472.584 106.958C440.675 93.7409 406.476 86.9382 371.938 86.9382C337.4 86.9382 303.201 93.7409 271.292 106.958C239.384 120.175 210.391 139.547 185.969 163.969L371.938 349.938L557.907 163.969Z" stroke="black" stroke-width="6" mask="url(#path-3-inside-3)"/>
    <mask id="path-4-inside-4" fill="white">
    <path d="M185.969 535.907C210.391 560.329 239.384 579.701 271.292 592.918C303.201 606.135 337.4 612.938 371.938 612.938C406.476 612.938 440.675 606.135 472.584 592.918C504.492 579.701 533.485 560.329 557.907 535.907L371.938 349.938L185.969 535.907Z"/>
    </mask>
    <path id='block-D' @click='playNote("s", true)' d="M185.969 535.907C210.391 560.329 239.384 579.701 271.292 592.918C303.201 606.135 337.4 612.938 371.938 612.938C406.476 612.938 440.675 606.135 472.584 592.918C504.492 579.701 533.485 560.329 557.907 535.907L371.938 349.938L185.969 535.907Z"  stroke="black" stroke-width="6" mask="url(#path-4-inside-4)"/>
    <circle @click='playGame' cx="372" cy="350" r="97" fill="#C4C4C4"/>
    </svg>





<!-- 
    <div @click='playNote("w", true)' id="block-A"></div>
    <div @click='playNote("a", true)' id="block-B"></div>
    <div @click='playNote("d", true)' id="block-C"></div>
    <div @click='playNote("s", true)' id="block-D"></div> -->
    <!-- <button id="block-5" @click='playGame'>click Me!</button> -->
  </div>
  <!-- why do I have to display state in some way for onUpdated to work? -->
  <ul :key="note" v-for="note in userNotes">
      <!-- <li>{{note}}</li> -->
  </ul>
</template>



<script>
import { defineComponent, onMounted, onUpdated, reactive, toRefs } from "vue";

import Akey from '../MusicNotes/piano-a_A_major.wav'
import Bkey from '../MusicNotes/piano-b_B_major.wav'
import Ckey from '../MusicNotes/piano-c_C_major.wav'
import Dkey from '../MusicNotes/piano-d_D_major.wav'

export default defineComponent({
  setup: () => {
    const state = reactive({
      availableNotes: ['w','a','d','s'],
      notes: [],
      userNotes: [],
      index: 0
    });

    onMounted(() => {
        window.addEventListener("keyup", e => {
            // this is neccessary because if the user hits anything other than the available notes
            // then it gets pushed into userNotes array and breaks the logic
            if(e.key === state.availableNotes.find(letter => letter === e.key)) playNote(e.key, true)
        })
    })

    const playNote = (key, user) => {
        if(user) state.userNotes.push(key)
        let audio;
        let note;
        switch(key){
            case "w":
            audio = new Audio(Akey);
            note = "A"
            break;
            case "a":
            audio = new Audio(Bkey);
            note = "B"
            break;
            case "d":
            audio = new Audio(Ckey);
            note = "C"
            break;
            case "s":
            audio = new Audio(Dkey);
            note = "D"
            break;
            default:
            return
        }
        let block = document.getElementById(`block-${note}`) || document.getElementById(`block-${note}-active`)
        block.id === `block-${note}` ? block.id = `block-${note}-active` : block.id = `block-${note}`
        audio.play()
    }

    const playGame = () => {
        createNewNote()
        state.userNotes = []
        state.index = 0
        let i = 0
        const id = setInterval(() => {
            playNote(state.notes[i])
            i++
            if(!state.notes[i]) clearInterval(id)
        }, 500)
    }

    const createNewNote = () => {
        let index = Math.floor(Math.random() * state.availableNotes.length)
        state.notes.push(state.availableNotes[index])
    };

    onUpdated(() => {
        if(!state.userNotes[state.index]) return
        if(state.userNotes[state.index] === state.notes[state.index]){
            state.index ++
        } else {
            window.alert('you lost')
        }
        if(state.index === state.notes.length){
            playGame()
        }
    })


    return {
        ...toRefs(state),
        playGame,
        playNote
    }
  },
});
</script>


<style>
.block-container {
  /* height: 80vh;
  width: auto;
  text-align: center;
  align-content: center;
  display: grid;
  gap: 1rem;
  margin: 15%;

  grid-template-areas:
    "a b c"
    "d e f"
    "g h i";

  grid-template-rows: repeat(3, 11.5rem);
  grid-template-columns: repeat(3, 11.5rem); */
}



#block-A {
  animation: red-pulse-block 1s forwards;
  grid-area: b;
  fill: red;
  color: white;
  justify-self: center;
  align-self: end;
}

#block-A-active {
  animation: red-pulse-active 1s forwards;
  grid-area: b;
  fill: lightcoral;
  color: white;
}
/* I hate this so much pls help */
@keyframes red-pulse-block {
    from{fill: lightcoral;}
    to{fill: red;}
}

@keyframes red-pulse-active {
    from{fill: lightcoral;}
    to{fill: red;}
}

#block-B {
  animation: blue-pulse-block 1s forwards;
  grid-area: d;
  fill: blue;
  color: white;
  justify-self: flex-end;
  align-self: center;
}

#block-B-active {
  animation: blue-pulse-active 1s forwards;
  grid-area: d;
  fill: lightblue;
  color: white;
}
/* I hate this so much pls help */
@keyframes blue-pulse-block {
    from{fill: lightblue;}
    to{fill: blue;}
}

@keyframes blue-pulse-active {
    from{fill: lightblue;}
    to{fill: blue;}
}

#block-C {
  animation: yellow-pulse-block 1s forwards;
  grid-area: f;
  fill: yellow;
  color: white;
  justify-self: flex-start;
  align-self: center;
}
#block-C-active {
  animation: yellow-pulse-active 1s forwards;
  grid-area: f;
  fill: lightyellow;
  color: white;
}

@keyframes yellow-pulse-block {
    from{fill: lightyellow;}
    to{fill: yellow;}
}

@keyframes yellow-pulse-active {
    from{fill: lightyellow;}
    to{fill: yellow;}
}

#block-D {
  animation: green-pulse-block 1s forwards;
  grid-area: h;
  fill: green;
  color: white;
  justify-self: center;
  align-self: flex-start;
}
#block-D-active {
  animation: green-pulse-active 1s forwards;
  grid-area: h;
  fill: lightgreen;
  color: white;
}

@keyframes green-pulse-block {
    from{fill: lightgreen;}
    to{fill: green;}
}

@keyframes green-pulse-active {
    from{fill: lightgreen;}
    to{fill: green;}
}

#block-5 {
  grid-area: e;
  color: black;
  justify-self: center;
  align-self: center;

}
</style>