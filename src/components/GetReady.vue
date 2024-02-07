<template>
    <h1>Get Ready!</h1>
</template>

<script>

export default {
    components: { },
    props: ["delay"],
    data() {
        return {
        }
    },
    mounted() {
        setTimeout( () => {
            this.$emit("runmain")
        }, this.delay)
    },
    methods: {
    }
}
</script>
