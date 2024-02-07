<template>
    <img :src="imageURL" @click="checkGroup()">
</template>

<script>
export default {
    props: [ "tileData", "vPos", "hPos" ],
    methods: {
        checkGroup() {
            this.$emit('tileclicked', this.tileData.group, this.hPos, this.vPos)
        }
    },
    computed: {
        imageURL() {
            return require('../assets/' + this.tileData.group + this.tileData.variant + '.jpeg')
        }
    }
}
</script>
