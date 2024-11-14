<template>
    <div class="puzzle">
        {{  numberOne }} + {{ numberTwo }} =
        <input ref="puzzleAnswer" class="puzzleAnswer" type="number" v-model="answer" @keyup.enter="submitAnswer()"/>
        <button @click="submitAnswer()">Submit</button>
    </div>

    <p v-if="solved">Correct!</p>
</template>

<script>
    import { ref, onMounted} from 'vue' 

    export default {
        emits: ['addKey', 'deductHealth'],
        setup(props, { emit }){
            const solved = ref(false);
            const numberOne = ref(Math.floor((Math.random() * 100) + 1));
            const numberTwo = ref(Math.floor((Math.random() * 100) + 1));
            const answer = ref(null);
            const puzzleAnswer = ref(null);

            function submitAnswer(){
                if((numberOne.value + numberTwo.value)==answer.value){
                    solved.value = true;
                    emit('addKey');
                }else{
                    emit('deductHealth');
                }
            }

            onMounted(() => {
                puzzleAnswer.value.focus();
            });

            return { puzzleAnswer,solved, numberOne, numberTwo, answer, submitAnswer}
        }
}
</script>