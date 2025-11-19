<template>
    <div class="app">
        <CalendarMain :sharedDate="sharedDate" :locale="locale" @dateSelected="handleDateSelected" />
        <div class="app-info">
            <input class="app-info__field" name="date" type="text" :value="dateSelected" disabled />
        </div>
    </div>
</template>

<script setup lang="ts">
import CalendarMain from '@/components/Calendar/CalendartMain.vue';
import { ref } from 'vue';

const sharedDate = ref<string>('');  // можно передать строку в формате 'YYYY-MM-DD'
const locale = ref<'en-EN' | 'ru-RU'>('en-EN'); // локализация изменится, если передать 'ru-RU'
const dateSelected = ref<string>('');

const handleDateSelected = (date: string) => {
    dateSelected.value = date;
};
</script>

<style lang="scss">
body {
    font-family: Helvetica, sans-serif;
}

* {
    box-sizing: border-box;
}

::selection {
    background-color: #283466;
    color: white;
}

.app {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    gap: 24px;
    width: 300px;

    &-info {
        &__field {
            width: 100%;
            padding: 12px;
            font-size: 16px;
        }
    }
}
</style>
