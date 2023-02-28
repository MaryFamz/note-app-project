<template>
  <div class="landing-page" v-if="isPageOpen">
    <div class="logo">
      <img src="./components/icons/note-logo.png" alt="Mega-Notes">
      <h1>Mega Notes</h1>
    </div>
    <div class="main-button">
      <a href="#" @click.prevent="meganote">Take Notes</a>
    </div>
  </div>

  <!-- Main note part -->
  <div class="main-note" v-if="mainNoteVisible">
    <div class="top-page">
      <nav>
        <div class="second-logo">
          <img src="./components/icons/note-logo.png" alt="Mega-Notes">
          <h2>Mega Notes</h2>
        </div>
        <h3>Pen down some notes</h3>
      </nav>
      <button @click.prevent="toggleModal">+</button>
    </div>
    <div class="cards">
      <details class="card" v-for="{title, nbody, date, id} in noteContainer" :key="id">
        <summary class="card-title">{{ title }}</summary>
        <p :contenteditable="editNote" class="note">{{ nbody }}</p>
        <div class="card-bottom">
          <p class="date">{{ date.toLocaleString() }}</p>
          <div class="font-icons">
            <i class="fa-solid fa-edit" @click="edit()"></i>
            <i class="fa-solid fa-trash" @click="remove(id, $event)"></i>
          </div>
        </div>
      </details>
    </div>
    <div class="overlay" v-if="isModalOpen">
      <div class="note-box">
        <input placeholder="Note title" name="note-title" v-model="noteTitle"/>
        <textarea placeholder="Scribble down your notes here" name="note-body" id="note" cols="30" rows="10" v-model="noteBody"></textarea>
        <button @click.prevent="addNote">Add Note</button>
        <button @click.prevent="toggleModal">Close</button>
      </div>
    </div>
    <footer>
      <div class="footer-nav">
        <div class="facebook">
          <i class="fa-brands fa-facebook"></i>
          <h2>@meganotes</h2>
        </div>
        <div class="instagram">
          <i class="fa-brands fa-instagram"></i>
          <h2>@meganotes</h2>
        </div>
        <div class="twitter">
          <i class="fa-brands fa-twitter"></i>
          <h2>@meganotes</h2>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import {ref} from 'vue'
const isPageOpen = ref(true)
const mainNoteVisible = ref(false)
const isModalOpen = ref(false)
const noteTitle = ref('')
const noteBody = ref('')
const noteContainer = ref([])
const editNote = ref(false)

 function meganote(){
  alert("Welcome to Mega Notes!")
  isPageOpen.value = false
  mainNoteVisible.value = true
 }

 function toggleModal(){
  isModalOpen.value = !isModalOpen.value
 }

 function addNote(){
  const newNote = {
    title : noteTitle.value,
    nbody : noteBody.value,
    date : new Date(),
    id: Math.random()*1000000
  }
  noteContainer.value.push(newNote)

  noteTitle.value = " "
  noteBody.value = " "
  isModalOpen.value = false
  console.log(noteContainer.value);
 }

 function remove(id, event){
  console.log(event);   
  noteContainer.value = noteContainer.value.filter(item => item.id !== id)
 }

 function edit(){
  editNote.value   = !editNote.value
 }

</script>

<style scoped>
.landing-page{
  min-height: 40em;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}
.logo{
  display: grid;
  place-items: center;
}
.logo img{
  height: 20em;
}
.logo h1{
  font-size: 3em;
  font-weight: 200;
  -webkit-text-stroke: 2px #080808;
}

.main-button{
  border: 1px solid #ffffffde;
  border-radius: 5em;
  display: grid;
  place-items: center;
}

.main-button a{
  text-decoration: none;
  font-size: 3em;
  color: #ffffffde;
}

.main-button:hover{
  background-color: #172957;
  border: none;
}

.main-button a:hover{
  color: black;
}



.top-page{
  display: flex;
  justify-content: center;
  align-items: center;
}
.top-page button{
  position: fixed;
  border: 1px solid black;
  border-radius: 50rem;
  font-size: 3rem;
  padding: 0.5rem 1.5rem;
  z-index: 5;
  right: 10px;
}

.top-page button:hover{
  background-color: #4B5E97;
  color: white;
  border: 1px solid white;
}
.main-note{
  display: grid;
  place-items: center;
}

nav{
  background-color: #172957;
  position: fixed;
  top: 1px;
  height: 12em;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 0 20em ;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
}

.second-logo{
  display: flex;
  align-items: flex-end;
}

.second-logo img{
  height: 5em;
}

.second-logo h2{
  font-size: 20px;
}

nav h3{
  margin-top: 20px;
  font-size: 1.5em;
  color: #4B5E97;
}

.cards{
  display: grid;
  gap: 1rem;
  grid-template-columns: repeat(auto-fill, minmax(25rem, 1fr));
  margin-top: 8rem;
  padding: 2rem;
}

.card{
  background-color: #4B5E97;
  padding: 1rem;
  border-radius: 1rem;
}

.card-title{
  font-size: 1.6rem;
  font-weight: 700;
}

.card-bottom{
  margin-top: 1rem;
  display: flex;
  justify-content: space-between;
}

.note{
  font-size: 1.4rem;
}
.date{
  font-size: 1.2rem;
  font-style: italic;
  font-weight: 500;
}

.font-icons{
  display: flex;
  gap: 2rem;
  align-items: center;
  padding: 0 1rem;
}

.font-icons i{
  font-size: 1.6rem;
}

.font-icons i:nth-of-type(1):hover{
  transform: scale(-1);
}
.font-icons i:nth-of-type(2){
  color: tomato;
}

.font-icons i:nth-of-type(2):hover{
  color: red;
}


.overlay{
  position: fixed;
  inset: 0;
  background-color: #172957d2;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 5;
}

.note-box{
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.note-box input{
  padding: 1.5rem;
  border-radius: 5rem;
  font-size: 3rem;
  outline: none;
}

.note-box textarea{
  resize: none;
  border-radius: 1rem;
  font-size: 2rem;
  padding: 1.5rem;
  outline: none;
}

.note-box button{
  font-size: 2rem;
  border-radius: 1rem;
  border: none;
  color: white;
}

.note-box button:nth-of-type(1){
  background-color: #4B5E97;
}

.note-box button:nth-of-type(2){
  background-color: tomato;
}

.note-box button:nth-of-type(1):hover{
  background-color: navy;
}

.note-box button:nth-of-type(2):hover{
  background-color: red;
}

footer{
  background-color: #172957;
  position: fixed;
  bottom: 1px;
  height: 12em;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 0 10em;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}

.footer-nav{
  display: flex;
  gap: 5rem;
  color: white;
}

.footer-nav i{
  font-size: 3rem;
}
</style>
