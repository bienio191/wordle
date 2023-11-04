<template>
    <div class="board">
        <LetterRow
            :rowNumber=1
            :activeRowNo=activeLetterRow
            @wordCompleted="handleWordCompleted"
        />
        <LetterRow
            :rowNumber=2
            :activeRowNo=activeLetterRow
            @wordCompleted="handleWordCompleted"
        />
        <LetterRow
            :rowNumber=3
            :activeRowNo=activeLetterRow
            @wordCompleted="handleWordCompleted"
        />
        <LetterRow
            :rowNumber=4
            :activeRowNo=activeLetterRow
            @wordCompleted="handleWordCompleted"
        />
        <LetterRow
            :rowNumber=5
            :activeRowNo=activeLetterRow
            @wordCompleted="handleWordCompleted"
        />
    </div>
</template>


<script>

    import LetterRow from '../components/LetterRow.vue';

    export default {
        components: { 
            LetterRow 
        },
        data() {
            return {
                activeLetterRow: 1,
            }
        },
        computed: {
            //make sure there are all uppercase
            wordToGuess() {
                return ['S','N','A','K','E']
            },
        },
        methods: {
            
            //check if word exists in dictionary
            isInDictionary(word) {
                return true;
            },

            //return a word in string
            getStringWord(word) {
                let wordStr = "";
                word.forEach(element => {
                    wordStr += element.character; //concatenate all the chars into one string
                });
                return wordStr;
            },

            evaluateWord(word) {
                //convert to string
                const wordStr = this.getStringWord(word);
                //if not in dictionary, return false
                if(!this.isInDictionary(wordStr)) return false;
                
                word.forEach((element, index) => {
                    //if character is a perfect fit
                    if(element.character.toUpperCase() === this.wordToGuess[index].toUpperCase()) {
                        element.status = 'OK';
                        return;
                    }
                    //if character's position is wrong
                    if (this.wordToGuess.includes(element.character.toUpperCase())) {
                        element.status = 'WP';
                        return;
                    } 
                    //in any other case, character is not part of the word to guess
                    element.status = 'NOK';
                });
                return true;
            },

            handleWordCompleted(rowNumber, word) {
                const isValid = this.evaluateWord(word);
                if(isValid) {
                    this.activeLetterRow = rowNumber + 1;
                }
            },
        },
    }
</script>

<style>

    *,
    *::before,
    *::after {
        box-sizing: border-box;
    }

    body {
        font-size: 60px;
    }

</style>

