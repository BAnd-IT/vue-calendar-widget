<template>
    <div id="app">
        <CalendarMain :sharedDate="sharedDate" />
    </div>
</template>

<script setup lang="ts">
import CalendarMain from '@/components/Calendar/CalendartMain.vue';
import { ref } from 'vue';

const sharedDate = ref(null);
</script>

<style>
body {
    font-family: Helvetica, sans-serif;
}

* {
    box-sizing: border-box;
}
</style>
