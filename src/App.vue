<script>
    import Calendar from "./components/Calendar.vue";

    export default {
        components: {
            Calendar
        },
        data() {
            return {
                events: [
                    {
                        id: 1,
                        name: 'Событие 1',
                        date: new Date(),
                        type: 'red'
                    },
                    {
                        id: 2,
                        name: 'Событие 2',
                        date: 1651602387,
                        type: 'green'
                    },
                    {
                        id: 3,
                        name: 'Событие 3',
                        date: 1654280787,
                        type: 'orange'
                    },
                    {
                        id: 4,
                        name: 'Событие 4',
                        date: 1651859995,
                        type: 'orange'
                    },
                    {
                        id: 5,
                        name: 'Коммуникационный дизайн среды',
                        date: 1651863595,
                        type: 'red'
                    },
                ]
            }
        }
    }
</script>

<template>
    <calendar :events="events" />
</template>

<style>
</style>
