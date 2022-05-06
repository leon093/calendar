<script>
    import EventBlock from "./EventBlock.vue";
    import moment from 'moment';
    import 'moment/locale/ru';

    export default {
        name: "Calendar",
        components: {
            EventBlock
        },
        props: {
            events: Array
        },
        data() {
            return {
                dateCont: moment(),
                dayNames: [
                    'Понелельник',
                    'Вторник',
                    'Среда',
                    'Четверг',
                    'Пятница',
                    'Суббота',
                    'Воскресенье'
                ],
                types: {
                    orange: 'calendar__event--orange',
                    green: 'calendar__event--green',
                    red: 'calendar__event--red',
                },
                isShow: false
            }
        },

        computed: {
            startDay() {
                return this.dateCont.clone().startOf('month').startOf('isoWeek');
            },
            endDay() {
                return this.dateCont.clone().endOf('month').endOf('isoWeek');
            },
            day() {
                return this.startDay.clone().subtract(1, 'day');
            },
            days() {
                let calendar = []
                while (this.day.isBefore(this.endDay, 'day')) {
                    calendar.push(
                        Array(7)
                            .fill(0)
                            .map(() => this.day.add(1, 'day').clone())
                    );
                }
                return calendar;

            },
            month() {
                return this.dateCont.format('MMMM');
            },
            year() {
                return this.dateCont.format('YYYY');
            },
            isCurrentYear() {
                return moment().isSame(this.year, 'year');
            }
        },
        methods: {
            isToday(day) {
                return moment().isSame(day, 'day');
            },
            isWeekend(day) {
                return day.day() === 6 || day.day() === 0;
            },
            isPrevDay(day) {
                return moment().isAfter(day, 'day');
            },
            nextMonth() {
                this.dateCont = moment(this.dateCont).clone().add(1, 'month');
            },
            prevMonth() {
                this.dateCont = moment(this.dateCont).clone().subtract(1, 'month');
            },
            eventsFilter(day) {
                return this.events
                    .filter(event => event.date >= day.format('X') && event.date <= day.clone().endOf('day').format('X'));
            }
        }
    }
</script>

<template>
    <div class="calendar">
        <div class="calendar-nav">
            <div class="calendar-nav__col">
                <button class="calendar-nav__btn-prev" @click="prevMonth">
                    <i class="arrow-left-icon"></i>
                </button>
            </div>
            <div class="calendar-nav__col">
                <div class="calendar-nav__name">
                    {{ month }} {{ !isCurrentYear ? year : '' }}
                </div>
            </div>
            <div class="calendar-nav__col">
                <button class="calendar-nav__btn-next" @click="nextMonth">
                    <i class="arrow-right-icon"></i>
                </button>
            </div>
        </div>

        <div class="calendar__day-names">
            <div class="calendar__day-name"
                 v-for="(dayName, i) in dayNames"
                 :key="i">
                {{ dayName }}
            </div>
        </div>

        <div class="calendar__container">
            <div class="calendar__week"
                 v-for="(week, i) in days"
                 :key="i">

                <div class="calendar__day-number"
                     :class="{'calendar__today': isToday(day), 'calendar__weekend': isWeekend(day), 'calendar__prev': isPrevDay(day)}"
                     v-for="(day, ind) in week" :key="ind">

                    {{ day.format('D') }}
                    <div class="calendar__events">
                        <div class="calendar__event"
                             :class="types[event.type]"
                             v-for="event in eventsFilter(day)"
                             :key="event.id">

                            <EventBlock :class-type-event="types[event.type]" :event="event" />
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped></style>