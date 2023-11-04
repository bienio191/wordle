<template>
    <div class="row flex-container" :class="stateClass">
        <LetterBox
            ref="lb0"
            :position=0
            :status="word[0].status"
            @characterChanged="characterChanged"
        />
        <LetterBox
            ref="lb1"
            :position=1
            :status="word[1].status"
            @characterChanged="characterChanged"
        />
        <LetterBox
            ref="lb2"
            :position=2
            :status="word[2].status"
            @characterChanged="characterChanged"
        />
        <LetterBox
            ref="lb3"
            :position=3
            :status="word[3].status"
            @characterChanged="characterChanged"
        />
        <LetterBox
            ref="lb4"
            :position=4
            :status="word[4].status"
            @characterChanged="characterChanged"
        />
    </div>
</template>


<script>
    import LetterBox from '../components/LetterBox.vue';
    import { ref } from 'vue';

    export default {
        components: { LetterBox },
        props: {
            rowNumber: Number,
            activeRowNo: Number,
        },
        computed: {
            //check if row is active
            isActive() {
                return this.rowNumber === this.activeRowNo; 
            },
            //calculate class based on isActive flag
            stateClass() {
                return this.isActive ? 'active' : 'inactive';
            },
        },
        data() {
            return {
                word: [
                    {
                        character: "",
                        status: "",
                    },
                    {
                        character: "",
                        status: "",
                    },
                    {
                        character: "",
                        status: "",
                    },
                    {
                        character: "",
                        status: "",
                    },
                    {
                        character: "",
                        status: "",
                    },
                ],
            }
        },
        watch: {
            //set the focus to the first (0-based) letter box on the row switch
            isActive(newValue, oldValue) {
                if(newValue == true) {
                    this.focusInput(0);
                };
            },
        },
        mounted() {
            //set the focus to the first (0-based) letter box on the page load
            this.focusInput(0);
        },
        methods: {
            characterChanged(position, newValue) {
                this.word[position].character = newValue;
                //if there are not empty characters, so the word is completed, inform the parent component
                if(this.word.filter(item => item.character === '').length === 0) {
                    this.handleWordCompletion();
                }
                //switch focus to the next letter box
                this.focusInput(position+1);
            },

            handleWordCompletion() {
                this.$emit('wordCompleted', this.rowNumber, this.word);
            },

            //switch focus to the input element within a letter box of a $position
            focusInput(position) {
                if(this.isActive) {
                    console.log('focusInput: ' + position);
                    switch(position) {
                        case 0:
                            this.$refs.lb0.$el.firstChild.focus();
                            break;  
                        case 1:
                            this.$refs.lb1.$el.firstChild.focus();
                            break;  
                        case 2:
                            this.$refs.lb2.$el.firstChild.focus();
                            break;  
                        case 3:
                            this.$refs.lb3.$el.firstChild.focus();
                            break;  
                        case 4:
                            this.$refs.lb4.$el.firstChild.focus();
                            break;  
                    }
                }
            },
        },
    }

</script>

<style>
    .flex-container {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
    }

    .row {
        margin: 10px 0px;
    }

    .active div {
        pointer-events: all;
        background-color: lightgray;
    }

    .inactive {
        pointer-events: none;
    }
</style>

