<template>
    <div class="questions-ctr">
        <div class="progress">
            <div class="bar" :style="{ width: `${progressPercent}%` }"></div>
            <div class="status">
                {{ questionsAnswered }} out of {{ questions.length }} questions
                answered
            </div>
        </div>
        <transition-group name="fade">
            <div
                class="single-question"
                v-for="(question, qIdx) in questions"
                :key="question.q"
                v-show="shouldShowQuestion(qIdx)"
            >
                <div class="question">{{ question.q }}</div>
                <div class="answers">
                    <div
                        class="answer"
                        v-for="answer in question.answers"
                        :key="answer.text"
                        @click.prevent="selectAnswer(answer.is_correct)"
                    >
                        {{ answer.text }}
                    </div>
                </div>
            </div>
        </transition-group>
    </div>
</template>

<script>
export default {
    name: 'Questions',
    props: {
        questions: {
            type: () => Array,
        },
        questionsAnswered: {
            type: Number,
        },
    },
    emits: ['answer-selected'],
    methods: {
        shouldShowQuestion(questionIndex) {
            return this.questionsAnswered === questionIndex;
        },
        selectAnswer(isCorrect) {
            this.$.emit('answer-selected', isCorrect);
        },
    },
    computed: {
        progressPercent() {
            return (this.questionsAnswered / this.questions.length) * 100;
        },
    },
};
</script>

<style></style>
