<template>
    <header @keydown.esc="header=true" class="header">
        <Transition name="header">
        <div class="header__content" v-show="header == true">
            <button class="header__lang">ru</button>
            <h1 class="header__title">Заметки</h1>
            <button @click="header = false" class="header__search">
                <img src="../assets/img/search.svg" alt="">
            </button>
        </div>
        </Transition>
        <Transition name="header">
        <div class="header__form" v-show="header == false">
            <button @click="header = true, search=''" class="header__back">
                <img src="../assets/img/back.svg" alt="">
            </button>
            <input v-model="search" type="text" class="header__input container" placeholder="Поиск...">
            <button @click="search = ''" class="header__close">
                <img src="../assets/img/close.svg" alt="">
            </button>
        </div>
        </Transition>
    </header>
</template>

<script>

    export default {
        data(){
            return {
                header: true,
                search: ''
            }
        },
        created(){
            window.addEventListener('keyup', (e)=>{
                if (e.key == 'Escape') {
                    this.header = true
                }
            })
        },
        watch: {
            search(val){
                this.$emit('getSearch', val)
            }
        }
    }
</script>

<style lang="scss">
.header-enter-active,
.header-leave-active {
  transition: 0.5s ease;
}

.header-enter-from,
.header-leave-to {
  opacity: 0;
  transform: translateY(-100px);
}

.header {
    background: #F3EDF7;
    box-shadow: 0px 1px 3px 0px rgba(0, 0, 0, 0.30), 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
    padding: 14px 16px;
    height: 64px;
    position: relative;
    
    &__content, &__form {
        display: flex;
        justify-content: space-between;
        align-items: center;
        position: absolute;
        left: 16px;
        right: 16px;
        top: 14px;
        bottom: 14px;
    }
    
    &__lang {
        color: #1C1B1F;
        font-size: 22px;
        line-height: 28px; 
        text-transform: uppercase;
    }
    
    &__title {
        color: #1C1B1F;
        text-align: center;
        font-size: 22px;
        font-weight: 400;
        line-height: 28px; 
    }
    
    &__input {
        line-height: 20px;
        
        &::placeholder {
            color: #9D9D9D;
        }
    }
}
</style>