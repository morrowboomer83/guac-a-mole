<template>
  <p>Moles are infesting your garden like bugs infest your software!</p>
  <p>Click on the little mole to scare it away.</p>
  <p>Don't click on any guacamole or avocado!</p>
  <button @click="this.$emit('getready')">Play Game</button>
</template>

<script>
export default {
    components: { },
    emits: ["getready"],
    data() {
        return {
        }
    },
    methods: {
    }
}
</script>
