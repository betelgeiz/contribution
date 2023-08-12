<template>
    <div class="contribution-graph">
        <div class="weeks">
            <div class="week">
                <div class="day info"></div>
                <div class="day info"></div>
                <div class="day info">Пт</div>
                <div class="day info"></div>
                <div class="day info">Ср</div>
                <div class="day info"></div>
                <div class="day info">Пн</div>
            </div>
            <div v-for="(week, weekIndex) in weeks" :key="weekIndex" class="week">

                <div v-for="(day, dayIndex) in week" :key="dayIndex" class="day" :class="day.active">

                    <MyTooltip v-if="day.tooltip" :content="`${day.tooltip} contributions`" :dateTooltip="day.tooldtipDate"
                        :leftPosition="tooltipPosition(weekIndex)">
                    </MyTooltip>

                </div>
            </div>
        </div>

    </div>
    <div class="information">

        <div class="weeks">
            <span>Меньше</span>
            <div class="week">
                <div class="day"></div>
            </div>
            <div class="week">
                <div class="day firstLvl"></div>
            </div>
            <div class="week">
                <div class="day secondLvl"></div>
            </div>
            <div class="week">
                <div class="day thirdLvl"></div>
            </div>
            <div class="week">
                <div class="day fourthLvl"></div>
            </div>
            <span>Больше</span>
        </div>

    </div>
</template>
  
<script>
import { eachWeekOfInterval, subDays, format } from 'date-fns';
import { ru } from 'date-fns/locale'
import ky from 'ky';
import MyTooltip from './MyTooltip.vue';

export default {
    data() {
        return {
            contributions: [],
        };
    },
    components: {
        MyTooltip,
    },
    created() {
        this.getListContributions()
    },
    computed: {
        weeks() {
            const currentDate = new Date(); // Текущая дата
            const pastDate = subDays(currentDate, 357); // Отнимаем 357 дней

            const weekStarts = eachWeekOfInterval({
                start: pastDate,
                end: currentDate,
            });

            const weeks = [];

            for (const weekStart of weekStarts) {
                const weekDays = [];
                for (let i = 0; i < 7; i++) {
                    const currentDay = subDays(weekStart, i);
                    const formattedDay = format(currentDay, 'y-MM-dd');
                    const tooldtipDate = format(currentDay, `eeee, MMMM dd, yyyy`, { locale: ru });
                    weekDays.push({
                        day: formattedDay,
                        active: this.contributions[formattedDay] ? this.bgColor(this.contributions[formattedDay]) : false,
                        tooltip: this.contributions[formattedDay],
                        tooldtipDate
                    });
                }

                weeks.push(weekDays);
            }
            console.log(weeks)
            return weeks;
        },


    },
    methods: {
        async getListContributions() {
            try {
                const data = await ky.post('https://dpg.gg/test/calendar.json', { json: { foo: true } }).json();
                this.contributions = data

            } catch (error) {
                console.log(error)
            }
        },
        bgColor(value) {
            if (value >= 1 && value <= 9)
                return 'firstLvl'
            else if (value >= 10 && value <= 19)
                return 'secondLvl'
            else if (value >= 20 && value <= 29)
                return 'thirdLvl'
            else if (value >= 30)
                return 'fourthLvl'
            return false
        },
        tooltipPosition(value) {
            if (value < 5)
                return 450
            else if (value > 46)
                return -350
            return 50
        }
    }
};
</script>
  
<style scoped>
.months {
  display: inline-block; /* Размещение элементов в одной строке */
}
.contribution-graph {
    display: flex;
    align-items: flex-end;
    margin-top: 20px;
}

.weeks {
    display: flex;
    gap: 6px;
}

.week {
    display: flex;
    flex-direction: column-reverse;
    gap: 2px;
}

.day {
    width: 20px;
    height: 20px;
    background-color: #e9e9e9;
    transition: background-color 0.3s;
    box-sizing: border-box;
}

.day:hover {
    border: 1px solid black;
}

.firstLvl {
    background-color: #abd3f0;
}

.secondLvl {
    background-color: #7da5c6;
}

.thirdLvl {
    background-color: #6598c7;
}

.fourthLvl {
    background-color: #5588b9;
}

.info {
    background-color: #fff;
}

.information {
    margin: 2%;
}
</style>
  