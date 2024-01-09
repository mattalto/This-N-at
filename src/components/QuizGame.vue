<template>
    <main class="app">
        <h1>{{ name }}</h1>

        <section class="quiz" v-if="!quizCompleted">
            <div class="quiz-info">
                <div class="question">{{ getCurrentQuestion.question }}</div>

            </div>

            <div class="options">
                <label v-for="(option, index) in getCurrentQuestion.options" v-bind:key="index"
                    v-bind:class="`option 
                    ${getCurrentQuestion.selected === index ? getCurrentQuestion.answer === index ? 'correct' : 'incorrect' : ''}
                                                    } ${getCurrentQuestion.selected != null && index != currentQuestion.selected ? 'disabled' : ''}`">
                    <input type="radio" v-bind:name="getCurrentQuestion.index" :value="index"
                        v-model="getCurrentQuestion.selected" v-bind:disabled="getCurrentQuestion.selected"
                        v-on:change="setAnswer">

                    <span>{{ option }}</span>
                </label>
            </div>
            <div class="footer">
                <button v-on:click="nextQuestion" v-bind:disabled="!getCurrentQuestion.selected">
                    {{ getCurrentQuestion.index === questions.length - 1 ? 'Finish' : getCurrentQuestion.selected !== null ?
                        'Next Question' : 'Select an Option' }}
                </button>
                <div class="score">Score: {{ score }}/{{ questions.length }}</div>
            </div>
        </section>

        <section v-else>
            <h2>Thanks for playing This N'at!</h2>
            <p>Your total score is {{ score }} / {{ questions.length }}</p>
        </section>
    </main>
</template>

<script>
export default {
    data() {
        return {
            name: "This N'at",
            questions: [
                {
                    question: "Much like the author of this game, this cartoon dad is known for his love of pink frosted donuts with rainbow sprinkles.",
                    answer: 2,
                    options: [
                        "Peter Griffin",
                        "Stu Pickles",
                        "Homer Simpson",
                        "Hugh Neutron",
                    ],
                    selected: null
                },
                {
                    question: "This acclaimed HBO series infamously cut to black during the final scene, leaving viewers thinking that their power went out.",
                    answer: 1,
                    options: [
                        "The Wire",
                        "The Sopranos",
                        "White Lotus",
                        "Curb Your Enthusiasm",
                    ],
                    selected: null
                },
                {
                    question: "This bodybuilder is tied for the most Mr. Olympia titles won all-time.",
                    answer: 0,
                    options: [
                        "Ronnie Coleman",
                        "Arnold Schwarzenegger",
                        "Chris Bumstead",
                        "Dorian Yates",
                    ],
                    selected: null
                },
                {
                    question: "This popular rift in Thingvellir, Iceland exists due to the splitting of the North American and Eurasian tectonic plates.",
                    answer: 0,
                    options: [
                        "Silfra",
                        "Skogafoss",
                        "Harpa",
                        "Solfar",
                    ],
                    selected: null
                },
                {
                    question: "This composer skipped writing his 9th Symphony by titling it 'Das Lied von der Erde' (Song of the Earth) in hopes of avoiding the 'Curse of the Ninth', which was a belief that composers would often die after writing their 9th Symphony.",
                    answer: 2,
                    options: [
                        "Ludwig van Beethoven",
                        "Anton Webern",
                        "Gustav Mahler",
                        "Sergei Prokofiev",
                    ],
                    selected: null
                },

            ],
            quizCompleted: false,
            currentQuestion: 0,
        }
    },
    computed: {
        score() {
            let value = 0;
            this.questions.forEach((question) => {
                if (question.selected === question.answer) {
                    value++;
                }
            })
            return value;
        },
        getCurrentQuestion() {
            return this.questions[this.currentQuestion];
            //return this.question = this.currentQuestion;
            //return this.question;
        },
        setAnswer(event) {
            ((event) => {
                this.questions[this.currentQuestion].selected = event.target.value;
                event.target.value = null;
            })
        },
    },
    methods: {
        nextQuestion() {
            if (this.currentQuestion < this.questions.length - 1) {
                this.currentQuestion++;
            }
            else {
                this.quizCompleted = true;
            }
        }
    }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, Helvetica, sans-serif;
}

body {
    background-color: black;
    color: white;
}

.app {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    min-height: 100vh;
}

h1 {
    font-size: 3rem;
    margin-bottom: 2rem;
    background-image: linear-gradient(
        -225deg,
        #231557 0%,
        #44107a 29%,
        #ff1361 67%,
        #fff800 100%
    );
    background-size: auto auto;
    background-clip: border-box;
    background-size: 200% auto;
    color: #fff;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: textclip 2s linear infinite;
    display: inline-block;
}

@keyframes textclip {
    to {
        background-position: 200% center;
    }
}

.quiz {
    background-color: rgb(170, 170, 170);
    padding: 1rem;
    width: 100%;
    max-width: 640px;
    border-radius: 0.5rem;
}

.quiz-info {
    display: flex;
    justify-content: space-between;
    margin-bottom: 1rem;
}

.quiz-info .question {
    color: white;
    font-size: 1.25rem;
    text-shadow: 0 0 5px gray;
}

.option {
    display: block;
    padding: 1rem;
    background-color: rgb(211, 211, 211);
    margin-bottom: 0.5rem;
    border-radius: 0.5rem;
    text-shadow: 0 0 10px gray;
    cursor: pointer;
}

.option:hover {
    background-color: rgb(101, 214, 86);
}

.option .correct {
    background-color: aqua;
}

.option .incorrect {
    background-color: red;
}

.option:last-of-type {
    margin-bottom: 0;
}

.option .disabled {
    opacity: 0.5;
}

.option input {
    display: none;
}



.footer button {
    appearance: none;
    outline: none;
    border: none;
    cursor: pointer;
    
    margin-top: 10px;
    padding: 0.5rem 1rem;
    background-color: #44107a;
    color: white;
    font-weight: 700;
    text-transform: uppercase;
    font-size: 1.25rem;
    border-radius: 0.5rem;
}

.footer .score {
    display: flex;
    justify-content: end;
    color: rgb(255, 252, 207);
    font-size: 1.25rem;
    text-shadow: #fff800 1px 0 10px;
}

button:disabled {
    opacity: 0.5;
}

h2 {
    font-size: 2rem;
    margin-bottom: 2rem;
    background-image: linear-gradient(
        -225deg,
        #231557 0%,
        #44107a 29%,
        #ff1361 67%,
        #fff800 100%
    );
    background-size: auto auto;
    background-clip: border-box;
    background-size: 200% auto;
    color: #fff;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: textclip 2s linear infinite;
    
}

p {
    font-size: 1.5rem;
    margin-bottom: 2rem;
    background-image: linear-gradient(
        -225deg,
        #231557 0%,
        #44107a 29%,
        #ff1361 67%,
        #fff800 100%
    );
    background-size: auto auto;
    background-clip: border-box;
    background-size: 200% auto;
    color: #fff;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: textclip 2s linear infinite;
}
</style>