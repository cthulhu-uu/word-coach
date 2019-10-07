<template>
    <nb-container>
        <nb-content padder>
            <view block 
                :class="['score-text-box', is_expert_mode? 'space-between' : 'justify-right']"
            >
                <nb-text :style="{
                            marginTop: 14
                        }" v-if="is_expert_mode">Time: {{time}}</nb-text>
                <view :style="{flex: 1, flexDirection: 'row', justifyContent: 'flex-end', width: '20%'}">
                    <nb-text
                        :style="{
                            marginTop: 14
                        }"
                    > Score  •
                    </nb-text>
                    <nb-text
                        :style="{
                            color: success_color,
                            marginTop: 10 + Math.cos(scoreBounce*Math.PI*2)*4
                        }">{{score}}</nb-text>
                </view>
            </view>
                
            <nb-card>
                <nb-card-item class="title-flex">
                    <nb-text class="question-text">Which word is <nb-text class="question-text italic">{{question.type}}</nb-text> to {{question.word}}?</nb-text>
                </nb-card-item>
                <nb-card-item>
                    <nb-button 
                        :bordered="!answered || !is_chosen(random_choices[0])" 
                        block 
                        :success="is_chosen(random_choices[0]) && is_correct(random_choices[0])" 
                        :danger="is_chosen(random_choices[0]) && !is_correct(random_choices[0])" 
                        class="button-answer" 
                        @press="submit(random_choices[0])">
                        <view :style="{
                            flexDirection: 'row',
                            justifyContent: 'center',
                        }">
                            <nb-text><nb-icon class="button-icon" v-if="is_chosen(random_choices[0])" 
                                :name="is_correct(random_choices[0]) ? 'check' : 'times' " type="FontAwesome" 
                            :style="{
                                fontSize: 9 + animBtnIcon*9,
                                opacity: Math.max(0, animBtnIcon*3-2),
                                color: 'white',
                            }"></nb-icon><nb-text v-if="is_chosen(random_choices[0])" :style="{
                                fontSize: 1+animBtnIcon*11,
                            }">{{ "    " }}</nb-text>
                            {{ random_choices[0] }}<nb-text v-if="is_chosen(random_choices[0])" :style="{
                                fontSize: 1+animBtnIcon*11,
                            }">{{ "   " }}</nb-text></nb-text> 
                        </view>
                    </nb-button>
                </nb-card-item>
                <!-- <nb-card-item block> -->
                    <nb-text block class="light-text centered-text">or</nb-text>
                <!-- </nb-card-item> -->
                <nb-card-item>
                    <nb-button 
                        :bordered="!answered || !is_chosen(random_choices[1])" 
                        block 
                        :success="is_chosen(random_choices[1]) && is_correct(random_choices[1])" 
                        :danger="is_chosen(random_choices[1]) && !is_correct(random_choices[1])" 
                        class="button-answer" 
                        @press="submit(random_choices[1])">
                        <view :style="{
                            flexDirection: 'row',
                            justifyContent: 'center',
                        }">
                            <nb-text><nb-icon class="button-icon" v-if="is_chosen(random_choices[1])" 
                                :name="is_correct(random_choices[1]) ? 'check' : 'times' " type="FontAwesome" 
                            :style="{
                                fontSize: 9 + animBtnIcon*9,
                                opacity: Math.max(0, animBtnIcon*3-2),
                                color: 'white',
                            }"></nb-icon><nb-text v-if="is_chosen(random_choices[1])" :style="{
                                fontSize: 1+animBtnIcon*11,
                            }">{{ "    " }}</nb-text>
                            {{ random_choices[1] }}<nb-text v-if="is_chosen(random_choices[1])" :style="{
                                fontSize: 1+animBtnIcon*11,
                            }">{{ "   " }}</nb-text></nb-text> 
                        </view>
                    </nb-button>
                </nb-card-item>
                <nb-card-item class="progress-container"
                    :style="{marginTop: dotMarginTop()}"
                >
                    <view
                        :style="{
                            width: 0,
                            height: 0,
                            borderWidth: dotWidth(i),
                            borderRadius: '50%',
                            borderColor: dot_color(i),
                            margin: 4.5,
                        }"
                        v-for="i in questions_per_level"
                        :key="i"
                    ></view>
                </nb-card-item>
            </nb-card>
        <nb-view padder :style="{flexDirection: 'row', justifyContent: 'center'}">
            <nb-button bordered @press="reset()">
                <nb-text>Reset</nb-text>
            </nb-button>
        </nb-view>
        </nb-content>
    </nb-container>
</template>

<script>
import React from "react";
import { Toast, Button } from "native-base";
import { Animated, Easing } from "react-native";
import c_list from "../assets/questions/store.json";
import e_list from "../assets/questions/expert-store.json";
import variables from "../native-base-theme/variables/commonColor";

var q_list = c_list;

export default {
    name: "GameScreen",
    props: {
        navigation: {
            type: Object
        },
    },
    data: {
        questions: [...q_list],
            //{type: "better_image"}
            //{type: "word_for_image"}
        question_n: Math.floor(Math.random()*q_list.length),
        answered_questions: [],
        score: 0,
        debug: "",
        answered: false,
        chosen: "",
        questions_per_level: 5,
        transitionPerc: 0,
        transitioning: false,
        dotGrowth: 0,
        scoreBounce: 0,
        scoreBounceTransitioning: false,
        animBtnIcon: 0,
        animBtnIconTransitioning: false,
        success_color: variables.brandSuccess,
        fail_color: variables.brandDanger,
        time: 0,
        timerid: []
    },
    computed: {
        question() {
            return this.questions[this.question_n]
        },
        random_choices() {
            let answers = Object.values(this.question.answers);
            let i = Math.floor(Math.random() * answers.length);
            return [answers.splice(i,1)[0], answers[0]];
        },
        is_expert_mode() {
            return this.navigation.getParam('expert');
        }
    },
    created() {
        if (this.is_expert_mode) {
            q_list = e_list;
            this.simple_timer();
            this.reset()
        }
    },
    methods: {
        stoptimer() {
            for (i = this.timerid.length-1; i >= 0; i--) {
                clearTimeout(this.timerid[i])
            }
        },
        lose() {
            this.stoptimer();
            alert("y o u l o s e");
        },
        run_timer() {
            this.time--;
            if(this.time==0) {
                this.lose();
            }
        },
        simple_timer() {
            this.time = 3;
            this.timerid = []
            for (i=3; i>0; i--) {
                this.timerid.push(setTimeout(this.run_timer, i*1000));
            }
        },
        countdown() {
            setTimeout(this.time, 3000);
        },
        is_chosen(choice) {
            return this.chosen == choice
        },
        is_correct(choice) {
            return choice == this.question.answers.correct;
        },
        dot_color(i) {
            if (i == this.answered_questions.length+1 && this.answered) {
                if (this.is_correct(this.chosen)) {
                    return this.success_color;
                } else {
                    return this.fail_color;
                }
            }
            return '#aaa';
        },
        next_question() {
            // reset state
            this.chosen = "";
            this.answered = false;
            this.transitioning = false;
            
            // add current question to answered list
            this.answered_questions.push(this.question);

            // remove quesiton from question pool
            this.questions.splice(this.question_n, 1);

            if (this.questions.length <= 1) {
                this.questions = [...q_list];
                // this.answered_questions = [];
            }

            if (this.answered_questions.length >= this.questions_per_level) {
                this.answered_questions = [];
            }

            // choose a new question
            this.question_n = Math.floor(Math.random()*this.questions.length);
            
            this.simple_timer()

            // success
            return true;
        },
        submit(choice) {
            if (!this.answered) {
                this.chosen = choice;
                let transitionTime = 1300;
                this.stoptimer();
                if (this.is_correct(choice)) {
                    this.score += 100;
                    this.animate('scoreBounce', transitionTime/5)
                } else if(this.is_expert_mode) {
                    this.score = 0;
                    this.lose();  
                }
                this.answered = true;

                this.animate('animBtnIcon', transitionTime/6)
                setTimeout(() => {
                    this.transitioning = true;
                    this.animate('dotGrowth', transitionTime/6)
                }, transitionTime * 5/6);
                setTimeout(() => {
                    this.next_question();
                }, transitionTime);
            }
        },
        animate(field, transitionTime, steps=16) {
            this[field] = 0;
            let transField = field + 'Transitioning'
            this[transField] = true;

            for(let i = 0; i < steps; i++) {
                setTimeout(() => {
                    this[field] = (i+1)/steps;
                    if (i + 1 > steps) {
                        this[transField] = false;
                    }
                }, transitionTime/steps * i);
            }

        },
        dotWidth(i) {
            let current = this.answered_questions.length+1;
            let next = current + 1
            if (!this.transitioning) {
                if (i==current) {
                    return 5
                }
            }
            else {
                if (i == next) {
                    return 3+2*this.dotGrowth
                } else if (i == current) {
                    return 5-2*this.dotGrowth 
                }
            }
            return 3
        },
        dotMarginTop() {
            if (this.transitioning) {
                return Math.max(Math.sin(this.dotGrowth*Math.PI*2),0)*1
            }
        },
        reset() {
            this.score = 0;
            this.answered = false;
            this.chosen = "";
            this.questions = [...q_list];
            this.answered_questions = [];
            this.question_n = Math.floor(Math.random()*this.questions.length);
            this.lost = false;
            if (this.is_expert_mode) {
                this.stoptimer()
                this.simple_timer()
            }
        }
    }
}
</script>

<style scoped>
.container {
  flex: auto;
  background-color: white;
  align-items: center;
  justify-content: center;
}
.button-answer {
    width: 100%;
}

.answer-text {
    text-align: center;
    align-self: center;
}

.score-text-box {
    flex: 1;
    flex-direction: row;
    margin-right: 5;
    margin-top: 20;
    margin-bottom: 5;
    text-align: right;
    color: #aaa;
}
.space-between {
    justify-content: space-between;
}
.justify-right {
    justify-content: flex-end;
}
.question-text {
    font-size: 20px;
    font-weight: bold;
    text-align: center;
    width: 100%;
}
.italic {
    font-style: italic;
}
.light-text {
    color: #aaa;
}
.border {
    border-width: 2px;
    border-color: #333;
}
.progress-container {
    display: flex;
    flex: 1;
    flex-direction: row; 
    justify-content: center;
    align-items: center; 
    height: 40px;
}
.progress-dots {
    vertical-align: center;
    border-width: 2px;
    width: 20; 
    height: 45;
    text-align: center;
    margin: 0;
    padding: 0;
    border-color: #2f2;
}
.centered-text {
    text-align: right;
    margin-left: auto;
    margin-right: auto;
    /* align-self: center; */
}
.regular-ass-button {
    width: 0;
}

.title-flex {
    display: flex;
    height: 80px;
    
}
.timer-text-box{
    flex: 1;
    flex-direction: row;
    margin-left: 5;
    margin-top: 20;
    margin-bottom: 5;
    text-align: left;
    color: #aaa;
    justify-content: flex-start;

}




</style>