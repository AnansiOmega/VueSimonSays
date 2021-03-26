<template>
  <div class="block-container">
    <div @click='playNote("w", true)' id="block-A"></div>
    <div @click='playNote("a", true)' id="block-B"></div>
    <div @click='playNote("d", true)' id="block-C"></div>
    <div @click='playNote("s", true)' id="block-D"></div>
    <button id="block-5" @click='playGame'>click Me!</button>
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
  height: 80vh;
  text-align: center;
  align-content: center;
  display: grid;
  gap: 1rem;

  grid-template-areas:
    "a b c"
    "d e f"
    "g h i";

  grid-template-rows: repeat(3, 1fr);
  grid-template-columns: repeat(3, 1fr);
}



#block-A {
  animation: red-pulse-block 1s forwards;
  grid-area: b;
  background-color: red;
  color: white;
}

#block-A-active {
  animation: red-pulse-active 1s forwards;
  grid-area: b;
  background-color: lightcoral;
  color: white;
}
/* I hate this so much pls help */
@keyframes red-pulse-block {
    from{background: lightcoral;}
    to{background: red;}
}

@keyframes red-pulse-active {
    from{background: lightcoral;}
    to{background: red;}
}

#block-B {
  animation: blue-pulse-block 1s forwards;
  grid-area: d;
  background-color: blue;
  color: white;
}

#block-B-active {
  animation: blue-pulse-active 1s forwards;
  grid-area: d;
  background-color: lightblue;
  color: white;
}
/* I hate this so much pls help */
@keyframes blue-pulse-block {
    from{background: lightblue;}
    to{background: blue;}
}

@keyframes blue-pulse-active {
    from{background: lightblue;}
    to{background: blue;}
}

#block-C {
  animation: yellow-pulse-block 1s forwards;
  grid-area: f;
  background-color: yellow;
  color: white;
}
#block-C-active {
  animation: yellow-pulse-active 1s forwards;
  grid-area: f;
  background-color: lightyellow;
  color: white;
}

@keyframes yellow-pulse-block {
    from{background: lightyellow;}
    to{background: yellow;}
}

@keyframes yellow-pulse-active {
    from{background: lightyellow;}
    to{background: yellow;}
}

#block-D {
  animation: green-pulse-block 1s forwards;
  grid-area: h;
  background-color: green;
  color: white;
}
#block-D-active {
  animation: green-pulse-active 1s forwards;
  grid-area: h;
  background-color: lightgreen;
  color: white;
}

@keyframes green-pulse-block {
    from{background: lightgreen;}
    to{background: green;}
}

@keyframes green-pulse-active {
    from{background: lightgreen;}
    to{background: green;}
}

#block-5 {
  grid-area: e;
  color: black;
}
</style>