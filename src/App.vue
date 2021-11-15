<template>

    <img alt="Vue logo" src="./assets/bwb_4.png">
    <h1>Survey</h1>
    <div class="page">
        <template v-for="(question, index) in questions" :key="index">
            <question v-if="index === current"
                :question="question.question"
                :choices="question.choices"
                :type="question.type"
                :selected="question.selected"
                :uid="index"
                :required="question.required"
                @changed="changed"
            />
        </template>
        <p>{{ status }}</p>
        <div class="btnPanel">
            <div class="grid">
            <button class="classBtn blueBtn" v-if="current > 0" @click="back">Back</button>
            </div>

            <div class="grid">
            <button class="classBtn greenBtn" v-if="current === questions.length - 1" @click="validate">Validate</button>
            </div>

            <div class="grid">
            <button class="classBtn blueBtn" v-if="current < questions.length - 1" @click="next">Next</button>
            </div>
        </div>
    </div>

    <h1 id="statistics">{{ "Statistics (" + surveyCount + " Votes)"}}</h1>
    <div class="stats" v-for="(question, index) in questions" :key="index">
        <h2>{{ question.question + " (" + (question.type === 'checkbox' ? 'multi' : 'single') + ")"}}</h2>
        <div v-for="(percent, index) in question.stat" :key="index">

            <h4>{{ question.choices[index] + " " + percent + "/" + surveyCount + " (" + calculate(percent, surveyCount) + "%)" }}</h4>
            <progress-bar class="bar" v-bind:value="calculate(percent, surveyCount)"/>

        </div>
    </div>

    <button class="classBtn blueBtn" @click="top">Survey</button>

</template>

<script>

    import Question from './components/Question.vue'
    import ProgressBar from './components/ProgressBar.vue'

    export default {
        name: 'App',
        components: {
            Question, ProgressBar
        },
        data() {
            return {
                current: 0,
                status: "",
                surveyCount: 0,
                questions: [
                    {
                        question: "Je préfère",
                        choices: ["Les légumes", "La viande", "Les kinder", "Les chats"],
                        type: "checkbox",
                        required: true,
                        selected: [],
                        stat: []
                    },
                    {
                        question: "Je vais voter pour",
                        choices: ["Tata", "Tete", "Titi", "Toto", "Tutu"],
                        type: "radio",
                        required: true,
                        selected: [],
                        stat: []
                    },
                    {
                        question: "Je préfère",
                        choices: ["PHP", "JS", "Python", "Assembleur", "Aucun"],
                        type: "checkbox",
                        required: true,
                        selected: [],
                        stat: []
                    },
                    {
                        question: "Si j'etais un volatile",
                        choices: ["Une grosse buse", "Un vilain vautour", "Un aigle majestueux", "Un dodo ronfleur"],
                        type: "radio",
                        required: true,
                        selected: [],
                        stat: []
                    },
                    {
                        question: "Plutot",
                        choices: ["Entendre ca que d'etre sourd", "Mourir debout que vivre allongé", "Vivre libre"],
                        type: "radio",
                        required: true,
                        selected: [],
                        stat: []
                    }
                ]
            }
        },
        methods: {
            validate() {

                if (this.isValid()) {

                    for (let idx in this.questions) {
                        let res = [];
                        if (typeof(this.questions[idx].selected) === "number") {
                            res = [this.questions[idx].selected];
                        } else {
                            res = Object.values(this.questions[idx].selected);
                        }

                        let tmp = Object.values(this.questions[idx].stat);
                        for (let i in res) {
                                tmp[res[i]] += 1;
                        }
                        this.questions[idx].stat = tmp;

                        this.questions[idx].selected = [];
                    }

                    this.current = 0;
                    this.surveyCount += 1;
                    window.open("#statistics", "_self");
                }
            },
            isValid() {
                let tmp = this.questions[this.current].selected;
                if (typeof(tmp) === "number" || tmp.length || !this.questions[this.current].required) {
                    this.status = "";
                    return true;
                } else {
                    this.status = "You must do a choice";
                    return false;
                }
            },
            next() {
                if (this.isValid()) { this.current += 1; }
            },
            back() {
                this.status = "";
                this.current -= 1;
            },
            changed(data) {
                this.questions[data.uid].selected = data.selected;
                this.status = "";
            },
            calculate(item, total) {
                let res = Math.floor((item / total) * 100);
                return  isNaN(res) ? 0 : res;
            },
            top() {
                window.open('#top', '_self');
            }
        },
        mounted() {
            for (let idx in this.questions) {
                if (this.questions[idx].stat.length === 0) {
                    this.questions[idx].stat = Array(this.questions[idx].choices.length);
                    let len = this.questions[idx].stat.length;
                    for (let i = 0 ; i < len ; i++) {
                        this.questions[idx].stat[i] = 0;
                    }
                }
            }
        }
    }

</script>

<style>

    .stats {
        border: 3px solid #2c3e50;
        padding-bottom: 25px;
        margin: 60px;
    }

    .bar {
        margin: 0px auto;
    }

    h4 {
        margin: 20px 0px 0px 0px;
    }

    p {
        height: 15px;
        color: #F04040;
    }

    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    .page {
        height: 400px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
    }

    .grid {
        width: 10%;
        margin: 16px;
    }

    .btnPanel {
        display: flex;
        justify-content: center;
    }

    .classBtn {
        padding: 7px 10px;
        font-size: 18px;
        border: 0px;
        opacity: 0.75;
        border-radius: 10px;
        color: #202020;
        text-decoration: none;
        text-align: center;
    }

    .classBtn:hover {
        opacity: 1.0;
    }

    .greenBtn {background-color: #40C040;}
    .blueBtn {background-color: #0080FF;}

</style>
