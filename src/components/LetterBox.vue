<template>
    <div class="box flex-container" :class="status">
        <input
            type="text"
            tabindex="-1"
            v-model="character"
        />
    </div>
</template>

<script>
    export default {
        props: {
            position: Number,
            status: String, //"", OK, NOK, WP (wrong position)
        },
        data() {
            return {
                character: '',
            }
        },
        methods: {
        },
        watch: {
            character(newValue, oldValue) {
                //not allow to pass multiple character into a singe box
                if(this.character.length > 1) {
                    this.character = oldValue;
                    return;
                } 

                this.$emit('characterChanged', this.position, this.character);
            }
        },
    }

</script>

<style>
    input {
        border: 1px solid black;
        background: linear-gradient(to left, #ccc 1px, transparent 0);
        width: 70px; 
        height: 100px;
        font-size: 60px;
        text-transform: uppercase;
        text-align: center; 
    }

    .flex-container {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .box {
        width: 70px;
        height: 100px;
        background-color: darkgray;
    }

    .OK {
        background-color: green;
    }

    .NOK {
        background-color: red;
    }

    .WP {
        background-color: yellow;
    }
</style>