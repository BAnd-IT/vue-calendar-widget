<template>
    <div class="app">
        <CalendarMain :sharedDate="sharedDate" @dateSelected="handleDateSelected" />
        <input class="app-input" type="text" :value="dateSelected" disabled>
    </div>
</template>

<script setup lang="ts">
import CalendarMain from '@/components/Calendar/CalendartMain.vue';
import { ref } from 'vue';

const sharedDate = ref<string>('');
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
    gap: 24px;
    width: 300px;

    &-input {
        padding: 12px;
        font-size: 16px;
    }
}
</style>
