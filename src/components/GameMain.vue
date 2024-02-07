<template>
    <table>
        <tr v-for="row in rows">
            <td v-for="col in row.cols">
                <Tile :tileData="col" :vPos="row.pos" :hPos="col.pos" @tileclicked="handleTileClick" />
            </td>
        </tr>
    </table>
</template>

<script>
import Tile from './Tile.vue'

export default {

    components: {
        Tile
    },

    data() {
        return {
            rows: [
                { pos: 1, cols: [ {pos: 1, occupied: false, group: "blank", variant: "1"}, {pos: 2, occupied: false, group: "blank", variant: "1"}, {pos: 3, occupied: false, group: "blank", variant: "1"}, {pos: 4, occupied: false, group: "blank", variant: "1"}, {pos: 5, occupied: false, group: "blank", variant: "1"} ] },
                { pos: 2, cols: [ {pos: 1, occupied: false, group: "blank", variant: "1"}, {pos: 2, occupied: false, group: "blank", variant: "1"}, {pos: 3, occupied: false, group: "blank", variant: "1"}, {pos: 4, occupied: false, group: "blank", variant: "1"}, {pos: 5, occupied: false, group: "blank", variant: "1"} ] },
                { pos: 3, cols: [ {pos: 1, occupied: false, group: "blank", variant: "1"}, {pos: 2, occupied: false, group: "blank", variant: "1"}, {pos: 3, occupied: false, group: "blank", variant: "1"}, {pos: 4, occupied: false, group: "blank", variant: "1"}, {pos: 5, occupied: false, group: "blank", variant: "1"} ] },
                { pos: 4, cols: [ {pos: 1, occupied: false, group: "blank", variant: "1"}, {pos: 2, occupied: false, group: "blank", variant: "1"}, {pos: 3, occupied: false, group: "blank", variant: "1"}, {pos: 4, occupied: false, group: "blank", variant: "1"}, {pos: 5, occupied: false, group: "blank", variant: "1"} ] }
            ],

            tilesVisible: 0
        }
    },

    methods: {
        showRandomTile() {
            // 1. randomize a position until an unoccupied is found
            var tilePosition = this.getRandomUnoccupied()

            // 2. randomize chance of guacamole (20%), set mole otherwise
            var tileGroup = this.randomizeGroup()

            // 3. randomize variant (1-5)
            var tileVariant = this.rndOneToN(5)

            // 4. change the tile at "tilePosition"
            //    - set tileGroup
            //    - set setVariant
            //    - set the position to occupied
            this.rows[tilePosition.v-1].cols[tilePosition.h-1] = {pos: tilePosition.h, occupied: true, group: tileGroup, variant: tileVariant}
            this.tilesVisible++

            // 5. if the new tile is guacamole, set a new random timeout (0.8 to 2.4 sec)
            if (tileGroup == "guacamole") {
                setTimeout(() => {
                    if (this.tilesVisible >= 20) {
                        this.$emit("gameover", true)
                    } else {
                        this.showRandomTile()
                    }
                }, 800 + Math.random() * 1600)
            }
            
        },

        rndOneToN(n) {
            return Math.ceil(Math.random() * n)
        },

        randomizeGroup() {
            return this.rndOneToN(5) == 1 ? "guacamole" : "mole"
        },

        getRandomUnoccupied() {
            // avoid an infinite loop!!!
            if (this.tilesVisible >= 20) {
                return
            }
            var isOccupied = true
            var position = null
            while (isOccupied) {
                position = { v: this.rndOneToN(4), h: this.rndOneToN(5) }
                isOccupied = this.rows[position.v-1].cols[position.h-1].occupied
            }
            return position
        },

        handleTileClick(clickedGroup, hPos, vPos) {
            if (clickedGroup == "guacamole") {
                this.$emit("gameover", false)
            }

            if ( clickedGroup == "mole") {
                // remove the mole
                this.rows[vPos-1].cols[hPos-1] = {pos: hPos, occupied: false, group: "blank", variant: "1"}

                if (this.tilesVisible > 0) {
                    this.tilesVisible--
                }

                // next tile appears after 0.3 to 1.1 sec
                setTimeout(() => {
                    this.showRandomTile()
                }, 300 + Math.random() * 800)
            }
        }

    },

    mounted() {
        this.showRandomTile()
    }

}
</script>
