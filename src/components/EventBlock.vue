<script>
    import moment from 'moment';

    export default {
        name: "EventBlock",
        props: {
            event: Object,
            classTypeEvent: String
        },
        data() {
            return {
                isShow: false
            }
        },
        methods: {
            dayTime(dayTime) {
                return moment(dayTime).format('hh:mm');
            },
            showEventHint() {
                const event = this.$refs.refEventText;
                const widthEvent = event.clientWidth;
                const fullWidthEvent = event.scrollWidth;

                if (fullWidthEvent > widthEvent) {
                    this.isShow = true;
                }
            }
        }
    }
</script>

<template>
    <div @mouseenter="showEventHint"
         @mouseleave="isShow = false"
         :class="{'calendar__hint-show' : isShow}"
         ref="eventName">

        <div class="calendar__text" ref="refEventText">
            {{ dayTime(event.date) }} {{ event.name }}
        </div>

        <div class="calendar__text-full" :class="classTypeEvent">
            {{ dayTime(event.date) }}
            {{ event.name }}
        </div>
    </div>
</template>

<style scoped>

</style>