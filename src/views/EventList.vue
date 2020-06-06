<template>
    <div>
        <h1 class="text-6xl font-bold mt-10">Event List</h1>
        <transition name="fadeRight">
        <EventCard/>
        </transition>

    </div>
</template>

<script>
    import EventCard from '../components/EventCard.vue';
    export default {
        name: "EventList",
        components: {
            EventCard
        }
    }
</script>

<style scoped>

</style>