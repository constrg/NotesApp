<script setup>
import { ref, computed } from 'vue';
import empty_box_icon from '../assets/img/empty_box_icon.png'

const showModal = ref(false);
const notes = ref([]);
const newNote = ref("");

const getRandomColor = () => {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
    notes.value.push({
        id: Math.floor(Math.random() * 1000000),
        text: newNote.value,
        date: new Date(),
        backgroundColor: getRandomColor()
    });
    showModal.value = false;
    newNote.value = "";
}

const deleteNote = (id) => {
    notes.value = notes.value.filter(note => note.id !== id);
};

const emptyNotes = computed(() => {
    return notes.value.length === 0;
})

const isAddDisabled = computed(() => {
    return newNote.value.trim() === "";
});

</script>

<template>
    <main>
        <div class="container">
            <div class="main_content">
                <div class="overlay" v-show="showModal">
                    <div class="modal">
                        <div class="modal-header">
                            <h1 class="modal-header-title">Add Your Note</h1>
                            <button class="close-button" @click="showModal = false">X</button>
                        </div>
                        <textarea v-model="newNote" name="note" cols="100" rows="10" class="note"
                            placeholder="Your Notes..."></textarea>
                        <button class="add-note-button" @click="addNote()" :disabled="isAddDisabled">Add Note</button>
                    </div>
                </div>

                <header>
                    <h1 class="header-title">Notes. 
                    <span class="rg">by RG</span></h1>
                </header>
                
                <div class="cards-container">
                    <div class="empty-notes" v-if="emptyNotes">
                        <img class="empty-box-icon" :src="empty_box_icon" alt="empty_box_icon" title="Empty Notes">
                    <p class="empty-notes-text">Empty Notes</p>
                    </div>
                    <div class="card" v-for="note in notes" :style="{backgroundColor: note.backgroundColor}" :key="note.id">
                    <div class="card-header">
                            <div class="delete-button-container">
                                <button class="detete-button" @click="deleteNote(note.id)">X</button>
                            </div>
                            <p class="card-text">{{ note.text }}</p>
                    </div>
                        <p class="card-date">{{ note.date.toLocaleDateString("en-US") }}</p>
                    </div>

                </div>

                
                <button class="add-button" @click="showModal = true" title="Add Note"><span class="add-note-text">Add Note</span>+</button>
            </div>
        </div>
    </main>
</template>

<style scoped>
main .main_content {
    padding-block: 20px 60px
}

main .main_content .add-button {
    padding: 10px;
    width: 50px;
    height: 50px;
    text-align: center;
    font-size: 20px;
    border: 1px solid #47eb07;
    background-color: #48ff00;
    cursor: pointer;
    border-radius: 10px;
    font-weight: 700;
    transition: all .3s;
    position: fixed;
    bottom: 20px;
    right: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    column-gap: 10px;
    box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
}

main .main_content .add-button .add-note-text 
{
    display: none;
    font-size: 16px;
    font-weight: 700;
}

main .main_content .add-button:hover 
{
    width: 150px;
    border-radius: 10px;
}

main .main_content .add-button:hover .add-note-text
{
    display: block;
}

main .main_content header {
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 1px solid rgba(182, 182, 182, 0.1);
}

main .main_content header .header-title {
    font-size: 50px;
    color: #fff;
    position: relative;
}

main .main_content header .header-title .rg
{
    font-size: 10px;
    display: block;
    position: absolute;
    bottom: 0;
}

main .main_content .cards-container {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    justify-content: center;
    gap: 20px;
    position: relative;
    width: 100%;
}

main .main_content .cards-container .empty-notes
{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    width: 100%;
    top: 0;
    position: absolute;
}
main .main_content .cards-container .empty-notes .empty-box-icon 
{
    width: 100%;
    max-width: 200px;
    position: relative;
}

main .main_content .cards-container .empty-notes .empty-notes-text 
{
    color: #808080;
    font-weight: 700;
    margin-top: -20px;
}

main .main_content .cards-container .card {
    width: 100%;
    min-height: 250px;
    background-color: #48ff00;
    border-radius: 20px;
    padding: 10px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    row-gap: 20px;
    transition: all .3s;
}

main .main_content .cards-container .card:hover 
{
    transform: rotate(2deg);
}

main .main_content .cards-container .card .card-header
{
    width: 100%;
}

main .main_content .cards-container .card .card-header .delete-button-container 
{
    display: flex;
    justify-content: flex-end;
}

main .main_content .cards-container .card .card-header .delete-button-container .detete-button 
{
    background-color: #da0101f2;
    width: 20px;
    height: 20px;
    color: #fff;
    font-size: 10px;
    border: none;
    cursor: pointer;
    border-radius: 50%;
    transition: all .3s;
}

main .main_content .cards-container .card .card-header .delete-button-container .detete-button:hover 
{
    transform: scale(.9);
}

main .main_content .cards-container .card .card-date {
    font-style: italic;
    font-size: 12px;
    font-weight: 700;
    border-top: 1px solid rgba(0, 0, 0, 0.1);
    padding-block: 5px;
}


main .main_content .overlay {
    position: fixed;
    width: 100%;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 999;
    padding: 20px;
}

main .main_content .overlay .modal {
    width: 100%;
    max-width: 500px;
    background-color: #fff;
    padding: 20px;
    border-radius: 20px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    position: relative;
}

main .main_content .overlay .modal .note {
    border: none;
    outline: none;
    margin-bottom: 10px;
    font-size: 16px;
}

main .main_content .overlay .modal .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
    margin-bottom: 20px;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
    padding-bottom: 10px;
    column-gap: 20px;
}



main .main_content .overlay .modal .modal-header .modal-header-title {
    font-size: 30px;
    font-weight: 700;
    color: #48ff00;
}

main .main_content .overlay .modal .modal-header .close-button {
    background-color: #da0101f2;
    width: 30px;
    height: 30px;
    color: #fff;
    font-size: 10px;
    border: none;
    cursor: pointer;
    border-radius: 50%;
    transition: all .3s;
    font-size: 16px;
}

main .main_content .overlay .modal .modal-header .close-button:hover 
{
    transform: scale(.9);
}

main .main_content .overlay .modal .add-note-button {
    padding: 10px;
    width: 100%;
    font-weight: 700;
    background-color: #48ff00;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    font-size: 16px;
}

@media (max-width: 800px) {
    main .main_content .cards-container {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 600px) {
    main .main_content .cards-container {
        grid-template-columns: 1fr;
    }
}
</style>