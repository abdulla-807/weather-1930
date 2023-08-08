<template>
    <div class="notes">
        <div class="container">
            <section class="notes__header">
                <h2 class="notes__title">{{ notes.length > 0 ? 'Все заметки' : 'Нет заметок' }}</h2>
                <button @click="grid = !grid" class="notes__btn">
                    <img v-if="grid" src="../assets/img/list.svg" alt="">
                    <img v-else src="../assets/img/grid.svg" alt="">
                    <span>{{ grid ? 'Список' : 'Сетка'}}</span>
                </button>
            </section>
            <section :class="{ active: !grid }" class="notes__list">
                <NotesCard 
                :grid="grid"
                v-for="item, index in notes" :key="item.id"
                :note="item"
                @delNote="$emit('delNote', item.id)"
                @changeNote="$emit('changeNote', item.id)"
                />
            </section>
        </div>
    </div>
</template>

<script>
    import NotesCard from './Notes-Card.vue';
    export default {
        components: {
            NotesCard
        },
        props:{
            notes: {
                default: [],
                type: Array
            }
        },
        data(){
            return {
                grid: true
            }
        }
    }
</script>

<style lang="scss">
.notes {
    margin-top: 30px;
    
    &__header {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    
    &__title {
        color: #323232;
        font-size: 22px;
        font-weight: 400;
        line-height: 28px; 
    }
    
    &__btn {
        border-radius: 16px;
        background: linear-gradient(0deg, rgba(103, 80, 164, 0.11) 0%, rgba(103, 80, 164, 0.11) 100%), #FFFBFE;
        box-shadow: 0px 1px 3px 0px rgba(0, 0, 0, 0.30), 0px 4px 8px 3px rgba(0, 0, 0, 0.15);
        padding: 16px 20px 16px 16px;
        display: flex;
        gap: 12px;
        align-items: center;
        color: #6750A4;
        font-size: 14px;
        font-weight: 500;
        line-height: 20px; 
        letter-spacing: 0.1px;
    }
    
    &__list {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 24px;
        margin-top: 30px;
        
        &.active {
            grid-template-columns: 1fr;
        }
    }
}
</style>