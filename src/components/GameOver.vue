<template>
    <div v-if="win">
        <h1>Congratulations!</h1>
        <p>You saved your avocado plantation from those pesky little moles!</p>
    </div>
    <div v-else>
        <h1>Game Over</h1>
        <p>Oh no! You clicked guacamole!</p>
    </div>
    <button @click="this.$emit('restart')">Restart</button>
</template>

<script>

export default {
    emits: ["restart"],
    props: [ "win" ]
}
</script>
