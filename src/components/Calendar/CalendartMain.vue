<template>
    <div class="calendar">
        <div class="calendar-header">
            <div class="calendar-header__control calendar-header__control--left" @click="previousMonth">◄</div>
            <div class="calendar-header__monthyear">{{ currentMonthYear }}</div>
            <div class="calendar-header__control calendar-header__control--right" @click="nextMonth">►</div>
        </div>
        <div class="calendar-days">
            <div v-for="(value, index) in weekDays" :key="index" class="calendar-days__day">
                {{ value }}
            </div>
        </div>
        <div class="calendar-dates js--calendar-dates" @click="handleDaySelect">
            <div v-for="day in calendarDays" :key="day.id" class="calendar-dates__date" :class="{
                'active': day.isSelected,
                'today': day.isToday,
            }" :data-date="day.dateString">
                {{ day.day }}
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue';

const emit = defineEmits(['dateSelected']);

const props = defineProps({
    sharedDate: [String],
});

const selectedDate = ref<Date | null>(null);
const currentDate = ref(new Date());

watch(() => props.sharedDate, (newDate) => {
    if (newDate) {
        const date = new Date(newDate);
        if (!isNaN(date.getTime())) {
            selectedDate.value = date;
            currentDate.value = new Date(date);
        }
    } else {
        selectedDate.value = null;
    }
}, { immediate: true });

const currentMonthYear = computed(() => currentDate.value.toLocaleDateString('en-En', {
    month: 'short',
    year: 'numeric'
}));

const weekDays = computed(() => {
    const baseDate = new Date(currentDate.value);
    baseDate.setDate(baseDate.getDate() - baseDate.getDay());

    const days = [];
    for (let i = 0; i < 7; i++) {
        const date = new Date(baseDate);
        date.setDate(date.getDate() + i);

        const dayName = date.toLocaleDateString('en-En', { weekday: 'short' });
        days.push(dayName);
    }

    return days;
});

const calendarDays = computed(() => {
    const year = currentDate.value.getFullYear();
    const month = currentDate.value.getMonth();

    const firstDayOfMonth = new Date(year, month, 1);
    const lastDayOfMonth = new Date(year, month + 1, 0);
    const firstDayOfWeek = firstDayOfMonth.getDay();
    const daysInMonth = lastDayOfMonth.getDate();

    const days = [];

    for (let i = 0; i < firstDayOfWeek; i++) {
        days.push({ date: null });
    }

    for (let day = 1; day <= daysInMonth; day++) {
        const date = new Date(year, month, day);
        days.push({
            id: `current-${day}`,
            day: day,
            date: date,
            dateString: formatDate(date),
            isToday: isToday(date),
            isSelected: isSelected(date)
        });
    }

    return days;
});

const isToday = (date: Date): boolean => {
    const today = new Date();
    return date.toDateString() === today.toDateString();
};

const isSelected = (date: Date): boolean => {
    if (!selectedDate.value) return false;
    return date.toDateString() === selectedDate.value.toDateString();
};

const formatDate = (date: Date): string => {
    const day = String(date.getDate()).padStart(2, '0');
    const month = String(date.getMonth() + 1).padStart(2, '0');
    const year = date.getFullYear();
    return `${year}-${month}-${day}`;
};

const previousMonth = () => {
    currentDate.value = new Date(currentDate.value.getFullYear(), currentDate.value.getMonth() - 1);
};

const nextMonth = () => {
    currentDate.value = new Date(currentDate.value.getFullYear(), currentDate.value.getMonth() + 1);
};

const handleDaySelect = (event: MouseEvent) => {
    const element = event.target as HTMLElement;
    const dateElement = element.closest('.calendar-dates__date') as HTMLElement;

    if (dateElement && dateElement.dataset.date) {
        const dateString = dateElement.dataset.date;
        const selected = new Date(dateString);

        selectedDate.value = selected;
        emit('dateSelected', dateString);
    }
};
</script>

<style lang="scss">
.calendar {
    padding: 4px;
    border: 1px solid #bebebe;

    * {
        user-select: none;
    }

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
            transition: background-color .15s ease-in-out;

            &.active {
                background-color: #283466;
                color: #fff;
            }

            &.today {
                font-weight: bold;
            }

            &:hover:not(.active):not(:empty) {
                background-color: #f0f0f0;
                cursor: pointer;
            }
        }
    }
}
</style>