<template>
        <div>
            <span v-for="n in numPuzzles" :key="n" ref="keyCollection">
                <img v-bind:src="numOfKeys >= n?'../assets/img/foundkey.png':'../assets/img/missingkey.png'">
            </span>
        </div>
        <Puzzle @addKey="updateKeyCollection()" @deductHealth="deductHealth()" :key="puzzleId"/>

</template>

<script>
    import Puzzle from './Puzzle.vue'
    import Item from './Item.vue'
    import { ref, useTemplateRef } from 'vue' 

    export default {
        name : 'Floor',
        components: {Puzzle, Item},
        emits: ['goToNextFloor', 'deductHealth'],
        setup(props, { emit }){
            const nextFloor = ref(false);
            const numOfKeys =  ref(0);
            const puzzleId = ref(1);
            const numPuzzles = ref(generateNoPuzzles());
            const keyCollection = useTemplateRef('keyCollection');

            function updateKeyCollection(){

                numOfKeys.value++;
                if(numPuzzles.value == numOfKeys.value){
                    nextFloor.value = true;
                    emit('goToNextFloor');

                }else{
                    puzzleId.value++;
                }
            }

            function deductHealth(){
                emit('deductHealth');
            }

            function generateNoPuzzles(){
                //Between 1 - 3 puzzle games per floor
                return Math.floor((Math.random() * 3) + 1)
            }

            return { nextFloor, numOfKeys, puzzleId, numPuzzles, keyCollection, updateKeyCollection, deductHealth, generateNoPuzzles}
        }
    }
</script>