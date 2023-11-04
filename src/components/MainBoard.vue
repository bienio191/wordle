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
        <div> {{ message }}</div>
    </div>
</template>


<script>

    import LetterRow from '../components/LetterRow.vue';
    import dict from '../assets/dict2.json';

    export default {
        components: { 
            LetterRow 
        },
        data() {
            return {
                activeLetterRow: 1,
                message: '',
            }
        },
        computed: {
            //make sure there are all uppercase
            dictionary() {
                return dict.filter(word => word.length === 5);
            },
            wordToGuess() {
                const index = Math.floor(Math.random() * this.dictionary.length);
                console.log('index: ' + index);
                const word = Array.from(this.dictionary[index].toUpperCase());
                console.log('word: ' + word);
                return word;
            },
        },
        methods: {
            //check if word exists in dictionary
            isInDictionary(word) {
                if(!this.dictionary.includes(word.toLowerCase())) {
                    return false;
                }
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

            //evalue if the word and return: 
            // - OK if the word is structurally correct, but not the final one, 
            // - NOK if this is not a world from dictionary
            // - END if this is the final word, so game over
            evaluateWord(word) {
                //convert to string
                const wordStr = this.getStringWord(word);
                //if not in dictionary, return false
                if(!this.isInDictionary(wordStr)) return "NOK";
                
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
                
                //check if all the letters are in the right place - then game over
                if(word.filter(item => item.status === 'OK').length === 5) {
                    return "END";
                }

                return "OK";
            },

            //handle the word completion
            handleWordCompleted(rowNumber, word) {
                const status = this.evaluateWord(word);

                if(status === "END") {
                    this.gameOverOK();
                } else if(status === "OK") {
                    this.activeLetterRow = rowNumber + 1;
                    if(this.activeLetterRow > 5) {
                        this.gameOverFail();
                    }
                } else if(status === "NOK") {
                    this.message = "Not in dictionary!";
                }
            },
            gameOverOK() {
                this.activeLetterRow = 99;
                this.message = "Congratulations!"
            },
            gameOverFail() {
                this.activeLetterRow = 99;
                this.message = "You lost! The word was: " + this.wordToGuess.join('');
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

