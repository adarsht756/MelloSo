<template>
    <h1 class="">This is {{ username }}'s page.</h1>
</template>

<script>
    export default {
        name: "User",
        props: [
            "username"
        ]
    }
</script>