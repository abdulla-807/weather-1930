<template>
  <div>
    <Header @getSearch="search = $event"/>
    <Notes 
    :notes="filterNotes"
    @delNote="delNote"
    @changeNote="changeNote"
    />
    <Modal 
    @closeModal="closeModal" 
    v-show="modalOpen"
    :currentId="currentId"
    @addNote="addNote"
    :edit="edit"
    :editNote="editNote"
    @editedNote="editedNote"
    />
    <AddButton @openModal="openModal"/>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Notes from './components/Notes.vue';
import Modal from './components/Modal.vue';
import AddButton from './components/Add-Button.vue';
import langs from './lang'
import { vasya, Petya } from './lang'
console.log(langs);
console.log(vasya);
console.log(Petya);

  export default {
    components: {
      Header,
      Notes,
      Modal,
      AddButton
    },
    data(){
      return {
        notes: [
          {
            id: 1,
            title: 'Vue',
            date: '07.03.2022',
            desc: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor'
          },
          {
            id: 2,
            title: 'React',
            date: '15.08.2000',
            desc: 'Lorem ipsum dolor sit amet'
          },
          {
            id: 3,
            title: 'Angular',
            date: '08.07.1981',
            desc: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor'
          },
        ],
        modalOpen: false,
        currentId: 1,
        edit: false,
        editNote: {},
        search: ''
      }
    },
    methods: {
      openModal(){
        this.modalOpen = true;
      },
      closeModal(){
        this.modalOpen = false
        setTimeout(() => {
          this.edit = false
        }, 500);
      },
      addNote(item){
        this.notes.push(item)
      },
      delNote(id){
        let index = this.notes.findIndex( (elem) => elem.id == id );
        this.notes.splice(index, 1)
        console.log(index);
      },
      getNotes(){
        let localNotes = localStorage.getItem('notes');
        if (localNotes) {
          this.notes = JSON.parse(localNotes); 
          let lastIndex = this.notes.length - 1;
          this.currentId = lastIndex >=0 ? this.notes[lastIndex].id + 1 : 1;
        }
      },
      changeNote(id){
        // let elem = this.notes.find((elem)=>{ return elem.id == id})
        let elem = this.notes.find((elem)=> elem.id == id)
        this.editNote = elem;
        this.modalOpen = this.edit = true;
      },
      editedNote(item){
        this.notes.forEach((elem)=>{
          if (elem.id == item.id) {
            elem.title = item.title;
            elem.desc = item.desc;
            elem.date = item.date;
          }
        })
      }
    },
    watch:{
      notes: {
        handler(val, oldVal) {
          localStorage.setItem('notes', JSON.stringify(this.notes))
        },
        deep: true
      }
    },
    created(){
      this.getNotes()
    }, 
    computed: {
      filterNotes(){
        let items = this.notes.filter((elem)=>{
          let result = elem.title.toLowerCase().includes(this.search.toLowerCase())
          return result;
        })
        return items
      }
    }
  }
</script>

<style scoped>

</style>