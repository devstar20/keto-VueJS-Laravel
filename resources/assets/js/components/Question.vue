<template>
    <div id="landing">
        <b-container class="bv-example-row bv-example-row-flex-cols">
            <b-row>
                <b-col class="box">
                    <Questions
                    v-if="questions.length"
                    :currentQuestion="questions[index]"
                    :next="(numTotal - 1) != index? onNext: to"
                    :increment="increment"
                    >
                    </Questions>
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>
<script>
import Questions from "./QuestionBox.vue";
import datas from "../data/questions.js"

export default {
    components: {
        Questions
    },
    data() {
        return {
            datas,
            questions: [],
            index: 0,
            numTotal: 0
        }
    },
    methods: {
        onNext(isNext) {
            this.index++
            return this.index
        },
        to() {
            this.$router.push({ path: '/summary' })
        },
        increment(isCorrect) {
            if (isCorrect) {
                this.numCorrect ++
            }
            // this.numTotal ++
        }
    },
    mounted: function() {
        for (const key in datas) {
            if (datas.hasOwnProperty(key)) {
                const element = datas[key];
                this.questions.push(element);
            }
            this.numTotal ++
        }
        console.log(this.questions);
    }
}
</script>
<style scoped>
.logo {
    max-width: 125px;
}
.box {
    align-items: center;
}
</style>