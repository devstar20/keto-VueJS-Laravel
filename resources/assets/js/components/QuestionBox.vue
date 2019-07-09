<template>
    <div class="desktop-container" id="desktop-container">
        <b-jumbotron class="question-box" v-if="routerParam != 'get_plan'">
            <template v-slot:header>
                <img src="../../images/images/icons/back.svg" alt="" class="btn-back" @click="goBack">
                <img src="../../images/images/logo1b26.svg" alt="" class="logo">
            </template>
            <b-progress height="5px" :value="progressValue" class="mb-2 progress"></b-progress>
            <b-container>
                <b-row>
                    <b-col class="question">{{ currentQuestion.question }}</b-col>
                </b-row>
                <b-list-group class="fancy-radio-holder">
                    <b-list-group-item @click="selectAnswer(index)" v-for="(answer, index) in answers" :key="index" class="list-question fancy-radio btn-single" v-bind:class="{active: isActiveStatus[index]}">
                        {{ answer }}
                        <p class="status"></p>
                        <p class="status-icon" v-if="selectedIndex == index">{{ questionStatus[1] }}</p>
                        <p class="status-icon" v-else>{{ questionStatus[0] }}</p>
                    </b-list-group-item>
                </b-list-group>
            </b-container>
        </b-jumbotron>
        <b-jumbotron class="question-box" v-else>
            <template v-slot:header>
                <img src="../../images/images/icons/back.svg" alt="" class="btn-back" @click="goBack">
                <img src="../../images/images/logo1b26.svg" alt="" class="logo">
            </template>
            <b-container>
                <b-row>
                    <b-col class="question">We've created a personalized Keto diet plan that will help you to achieve your goals.</b-col>
                </b-row>
                <b-row class="email-description" style="display:block;">Where should we send your plan?</b-row>
                <b-row class="input-holder">
                    <span id="suggest"></span>
                    <b-form-input placeholder="Enter your email address" id="email-value" type="email"></b-form-input>
                </b-row>
                <b-row class="error-msg"></b-row>
                <b-button variant="primary" class="btn-save-email" @click="to_subscribe">Click here to get your plan</b-button>
                <b-row class="email-promise" style="display: block;">We respect your privacy. We will never sell, rent or share your email address. That;s more than a policy, it's our personal guarantee!</b-row>
            </b-container>
        </b-jumbotron>
    </div>
</template>
<script>
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        planBox: Boolean
    },
    data() {
        return {
            selectedIndex: null,
            selectedAnswers: 0,
            progressValue: 14.3,
            isActiveStatus: [],
            multiSelection: false,
            routerParam: ''
        }
    },
    computed: {
        answers() {
            let answers = this.currentQuestion.answers;
            return answers;
        },
        questionStatus() {
            let questionStatus = ["-", "+"];
            return questionStatus
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index;
            
            var answerLists = document.getElementsByClassName('fancy-radio');
            console.log(answerLists);
            if (this.multiSelection == false) {
                this.selectedAnswers += 1;
                for (var i = 0; i < answerLists.length; i ++) {
                    console.log(answerLists[i]);
                    answerLists[i].classList.remove('active');
                }
                
                for (var i = 0; i < this.isActiveStatus; i ++) {
                    this.isActiveStatus[i] = '';
                }
            }
            else {
                this.progressValue = (this.next() + 1) * 14.3;
            }

            if (this.selectedAnswers == this.multiSelection) {
                this.progressValue = (this.next() + 1) * 14.3;
            }

            // this.progressValue = this.next() * 14.3;

            this.isActiveStatus[index] = true;
        },
        goBack() {
            this.$router.go(-1);
            this.isActiveStatus = false;
        },
        to_subscribe() {
            this.$router.push({path: '/plan'})
        }
    },
    mounted: function () {
        if (this.$route.query.plan != undefined )
            this.routerParam = this.$route.query.plan;
        console.log(this.routerParam);
        if (this.currentQuestion.multi_selects != undefined) {
            this.multiSelection = this.currentQuestion.multi_selects;
        }
    }
}
</script>
<style>
.question {
    color: #444;
    font-family: Karla;
    font-size: 25px;
    font-weight: 700;
    letter-spacing: .85px;
    line-height: 29px;
    margin-top: 20px;
    margin-bottom: 20px;
}
</style>
<style scoped>
h1 {
    margin-top: 0px;
}
.logo {
    max-width: 125px;
    max-height: 45px;
    padding-left: 15px;
}
.question-box {
    padding: 0px;
    border-radius: 9px;
    background-color: #fff;
    box-shadow: 0 4px 11px -2px rgba(86, 70, 143, .3);
    -webkit-box-shadow: 0 4px 11px -2px rgba(86, 70, 143, .3);
}
@media only screen and (min-width: 1000px) {
    .question-box {
        max-width: 600px;
        grid-column-start: 2;
        grid-column-end: five;
        grid-row-start: row1-start;
        grid-row-end: 3;
    }
}
.btn-back {
    position: relative;
}
.progress {
    background-color: #bbb5d2;
}
.list-question {
    margin-bottom: 15px;
    border-radius: 9px;
    border-width: 0px;
    /* box-shadow: 0 4px 11px -7px rgba(0, 0, 0, .2); */
    /* -webkit-box-shadow: 0 4px 11px -7px rgba(0, 0, 0, .3); */
}

.status_active {
    background-color: purple;
}
.icon_active {
    right: 7px;
}
</style>