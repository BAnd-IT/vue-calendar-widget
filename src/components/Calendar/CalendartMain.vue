<template>
    <div class="calendar">
        <div class="calendar-header">
            <div class="calendar-header__control calendar-header__control--left">◄</div>
            <div class="calendar-header__monthyear">Nov 2025</div>
            <div class="calendar-header__control calendar-header__control--right">►</div>
        </div>
        <div class="calendar-days">
            <div v-for="(value, index) in weekdays" :key="index" class="calendar-days__day">
                {{ value }}
            </div>
        </div>
        <div class="calendar-dates">
            <div v-for="value in 31" :key="value" class="calendar-dates__date">{{ value }}
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
const weekdays = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
</script>

<style lang="scss">
.calendar {
    display: inline-block;
    padding: 4px;
    border: 1px solid #bebebe;

    &-header {
        display: flex;
        align-items: center;

        &__monthyear,
        &__control {
            width: 40px;
            text-align: center;
        }

        &__monthyear {
            flex-grow: 1;
        }

        &__control {
            padding: 8px 0;
            cursor: pointer;
        }
    }

    &-days,
    &-dates {
        display: grid;
        grid-template-columns: repeat(7, 1fr);
    }

    &-days {
        font-size: 12px;

        &__day {
            width: 40px;
            padding: 8px 0;
            text-align: center;
        }
    }

    &-dates {
        font-size: 14px;

        &__date {
            width: 40px;
            padding: 8px 0;
            text-align: center;
            background-color: transparent;
            cursor: pointer;
        }
    }
}
</style>
