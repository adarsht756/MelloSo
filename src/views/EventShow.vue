<template>
    <h1 class="my-16 text-5xl font-bold">Showing Event {{ id }}</h1>
</template>

<script>
    export default {
        name: "EventShow",
        props: [
            'id'
        ]
    }
</script>