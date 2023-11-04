<template>
    <div class="row flex-container" :class="stateClass">
        <template v-for="item in word" :key="item.position">
            <LetterBox
                :ref="`lb${item.position}`"
                :position=item.position
                :status="item.status"
                @characterChanged="characterChanged"
                v-on:keyup.enter="submitWord"
            />
        </template>
        <button 
            class="submitBtn" 
            :class="isSubmitable ? 'submitable' : 'not-submitable'"
            @click="submitWord"
            >
            &#8594
    </button>
    <button 
            class="clearBtn" 
            :class="isActive ? 'submitable' : 'not-submitable'"
            @click="clearWord"
            >
            CLEAR
        </button>

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
                        position: 0,
                        character: "",
                        status: "",
                    },
                    {
                        position: 1,
                        character: "",
                        status: "",
                    },
                    {
                        position: 2,
                        character: "",
                        status: "",
                    },
                    {
                        position: 3,
                        character: "",
                        status: "",
                    },
                    {
                        position: 4,
                        character: "",
                        status: "",
                    },
                ],
                isSubmitable: false,
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
        methods: {
            characterChanged(position, newValue) {
                this.word[position].character = newValue;
                //if there are not empty characters, so the word is completed, inform the parent component
                if(this.word.filter(item => item.character === '').length === 0) {
                    this.handleWordCompletion();
                } else {
                    this.handleWordInCompletion();
                }

                //if newValue is empty, the word was cleared, so we don't want to handle the focus
                if(!newValue) return;
                //switch focus to the next letter box
                this.focusInput(position+1);
            },
            
            //show button to submit
            handleWordCompletion() {
                this.isSubmitable = true;
            },

            //hide button to submit
            handleWordInCompletion() {
                this.isSubmitable = false;
            },

            //on-click of the btn, emit the event
            submitWord() {
                if(this.isSubmitable) {
                    this.isSubmitable = false;
                    this.$emit('wordCompleted', this.rowNumber, this.word);
                }
            },

            //switch focus to the input element within a letter box of a $position
            focusInput(position) {
                console.log('focusInput: ' + position);
                if(this.isActive) {
                    switch(position) {
                        case 0:
                            this.$refs.lb0[0].$el.firstChild.focus();
                            break;  
                        case 1:
                            this.$refs.lb1[0].$el.firstChild.focus();
                            break;  
                        case 2:
                            this.$refs.lb2[0].$el.firstChild.focus();
                            break;  
                        case 3:
                            this.$refs.lb3[0].$el.firstChild.focus();
                            break;  
                        case 4:
                            this.$refs.lb4[0].$el.firstChild.focus();
                            break;  
                    }
                }
            },

            //clear the word on the click of the btn
            clearWord() {

                //clear the word 
                this.word.forEach((element)  => {
                    element.character = "";
                    element.status = "";
                });

                //stupid AF but I couldn't find any better working solution to update the child's components' character
                console.log(this.$refs);
                this.$refs.lb0[0].character = "";
                this.$refs.lb1[0].character = "";
                this.$refs.lb2[0].character = "";
                this.$refs.lb3[0].character = "";
                this.$refs.lb4[0].character = "";

                this.handleWordInCompletion();
                this.focusInput(0);
            }
        },
        mounted() {
            //set the focus to the first (0-based) letter box on the page load
            this.focusInput(0);
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

    .submitBtn {
        background-color: #4CAF50; /* Green */
        border: none;
        color: white;
        padding: 15px 32px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
    }

    .clearBtn {
        background-color: red; /* Green */
        border: none;
        color: white;
        padding: 15px 32px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
    }

    .submitable {
        visibility: visible;
    }   
    
    .not-submitable {
        visibility: hidden;
    }
</style>

