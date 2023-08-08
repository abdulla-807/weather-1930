<template>
    <Transition name="modal">
    <div @click="closeModal" class="modal">
        <div @click.stop="" class="modal__form">
            <h3 class="modal__title">{{ edit ? 'Изменить заметку' : 'Добавить заметку' }}</h3>
            <div class="modal__content">
                <label>
                    <span>Title</span>
                    <input v-model="title" type="text" placeholder="Title">
                </label>
                <label>
                    <span>Content</span>
                    <textarea v-model="desc" rows="1" placeholder="Content"></textarea>
                </label>
            </div>
            <div class="modal__controls">
                <button @click="closeModal" class="modal__btn modal__btn_red">Отмена</button>
                <button v-if="edit" @click="changeNote" class="modal__btn">изменить</button>
                <button v-else @click="addNote" class="modal__btn">Добавить</button>
            </div>
        </div>
    </div>
    </Transition>
</template>

<script>
    export default {
        props:{
            currentId: Number,
            edit: Boolean,
            editNote: Object
        },
        data(){
            return {
                title: '',
                desc: '',
                id: this.currentId
            }
        },
        methods: {
            closeModal(){
                this.$emit('closeModal')
            },
            addNote(){
                let title = this.title.trim();
                let desc = this.desc.trim();
                if(title.length > 0 && desc.length > 0){
                    let item = {
                        id: this.id++,
                        title,
                        date: new Date().toLocaleDateString(),
                        desc
                    }
                    this.closeModal()
                    this.title = this.desc = '';
                    this.$emit('addNote', item)
                }
            },
            changeNote(){
                let title = this.title.trim();
                let desc = this.desc.trim();
                if(title.length > 0 && desc.length > 0){
                    let item = {
                        id: this.editNote.id,
                        title,
                        date: new Date().toLocaleDateString(),
                        desc
                    }
                    this.closeModal()
                    this.title = this.desc = '';
                    this.$emit('editedNote', item)
                }
            }
        },
        watch: {
            edit(val){
                if(val){
                    this.title = this.editNote.title;
                    this.desc = this.editNote.desc;                    
                } else {
                    this.title = this.desc = '';
                }
            }
        }
    }
</script>

<style lang="scss">
.modal-enter-active,
.modal-leave-active {
  transition: 0.5s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
  transform: scale(1.5);
}

.modal {
    background: rgba(0, 0, 0, 0.35);
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    
    &__form {
        border-radius: 28px;
        background: linear-gradient(0deg, rgba(103, 80, 164, 0.11) 0%, rgba(103, 80, 164, 0.11) 100%), #FFFBFE;
        padding: 24px;
        max-width: 312px;
        width: 100%;
    }
    
    &__title {
        color: #1C1B1F;
        font-size: 24px;
        font-weight: 400;
        line-height: 32px;        
    }
    
    &__content {
        display: flex;
        flex-direction: column;
        
        label {
            @extend .modal__content;
            margin-top: 16px;
            border-radius: 4px 4px 0px 0px;
            background: #E7E0EC;
            padding: 8px 16px;
            border-bottom: 1px solid #1C1B1F;
        }
        
        span {
            color: #6750A4;
            font-size: 12px;
            line-height: 16px; 
            letter-spacing: 0.4px;
        }
        
        input, textarea {
            line-height: 24px; /* 150% */
            letter-spacing: 0.5px;
            
            &::placeholder {
                color: var(--color);
            }
        }
    }
    
    &__controls {
        display: flex;
        justify-content: end;
        margin-top: 24px;
        gap: 8px;
    }
    
    &__btn {
        padding: 10px 12px;
        color: #6750A4;
        font-size: 14px;
        font-weight: 500;
        line-height: 20px; /* 142.857% */
        letter-spacing: 0.1px;
        text-transform: uppercase;
        border-radius: 5px;
        transition: 0.5s;
        
        &:hover {
            background: #E6DDFF;
        }
        
        &_red {
            color: #CF1B1B;
            
            &:hover {
                background: #FFE1E1;
            }
        }
    }
}
</style>