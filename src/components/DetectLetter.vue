<template>
  <div>
    <Matrix v-model="currentLetterMatrix"></Matrix>
    <div
      class="letter-box"
      v-show="detectedLetters.length"
    >
      <div
        class="letter-box__item"
        v-for="letter in detectedLetters"
        :key="letter"
      >{{ letter }}</div>
    </div>
  </div>
</template>

<script>
  import Matrix from "./Matrix";

  export default {
    name: "DetectLetter",
    components: {Matrix},
    data () {
      return {
        currentLetterMatrix: [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
        letters: ["а","б","в","г","д","е","ж","з","и","к","л","м","н","о","п","р","с","т","у","ф","х","ц","ч","ш","щ","ъ","ы","ь","э","ю","я"],
        letterWeigths: [
          [-0.25, -2.75, -3.5, 4.75, -3.75, -0.25, -3.5, -2.75, -3.0, -1.5, -0.25, -3.75, -3.75, 4.25, 3.25, -0.75, -2.5, 1.5, -2.75, 2.0, -0.75, -3.5, -2.0, -8.0, 2.25],
          [0.0, -1.75, -1.75, 2.0, 0.25, 0.0, -1.25, -1.5, -2.0, -6.75, 0.0, -0.75, -1.75, 1.75, 0.75, -0.25, -1.25, -1.5, -5.0, 3.5, -0.25, -1.0, -1.75, 2.25, -1.5],
          [0.0, 0.25, -0.25, -0.25, -0.5, 0.0, 0.25, -0.5, 0.5, -0.5, 0.0, 0.25, 0.25, -0.25, 0.25, -0.5, 0.25, -1.0, -0.5, 0.0, -0.5, 0.75, 0.25, 0.5, -0.5],
          [-0.25, -0.5, 0.0, 0.25, 0.25, -0.25, -0.25, 0.25, -0.25, -0.25, -0.25, -0.25, 0.25, -1.0, -0.75, -0.25, -0.25, 0.0, -0.25, -0.75, -0.25, -0.25, 0.25, -0.5, -0.75],
          [-0.5, 0.25, 0.75, 0.0, -0.75, -0.25, -0.25, -0.5, -0.25, -0.75, -0.25, 0.25, -0.75, -0.5, -0.75, 1.25, 0.0, -0.5, 0.0, 0.0, 0.75, -0.5, -0.5, -0.75, 0.0],
          [-0.25, -0.75, -0.25, 0.5, 0.25, 0.0, -0.5, -0.25, -0.75, -2.5, 0.0, -0.25, -0.25, 2.5, -0.5, 0.0, -0.5, -0.5, -2.5, -2.0, -0.25, -0.5, -0.5, -0.5, 0.5],
          [0.5, -0.25, 0.0, -0.75, -0.25, -0.25, 0.25, 0.25, 0.25, -0.5, -0.25, -0.25, -0.25, -0.75, -0.25, -0.25, 0.5, 0.0, 0.5, -1.0, 0.25, 0.0, 0.0, -0.5, -0.25],
          [0.0, -0.25, 0.0, 0.25, -0.5, 0.0, -0.25, -1.0, -0.5, 0.0, 0.0, -0.25, 0.25, 0.0, 0.25, 0.0, -0.25, -1.0, -0.5, 0.0, 0.0, -0.5, 0.25, 0.25, 0.0],
          [-0.25, 0.0, -0.75, -0.75, 0.25, -0.25, 0.0, -0.5, -0.75, 0.25, -0.25, 0.25, -1.0, 0.0, 0.25, -0.25, 0.25, 0.0, -0.5, -0.5, -0.25, 0.25, -0.5, -0.25, -0.25],
          [-0.25, -0.25, -0.5, -1.75, -0.5, -0.25, -0.25, -0.75, -0.25, 0.25, -0.25, 0.0, 1.75, 0.0, -2.75, -0.25, 0.0, 0.5, 0.0, 0.5, -0.25, -0.5, -0.25, -1.25, -0.5],
          [-0.25, -0.25, -0.25, 0.5, 0.0, -0.25, -0.5, 0.0, -0.5, 0.0, -0.25, -0.25, -0.25, -1.0, -0.25, -0.25, -0.25, 0.0, 0.0, -0.25, -0.25, 2.75, -0.5, -0.5, -0.25],
          [0.25, -0.25, -0.25, -0.25, 0.0, 0.25, 0.0, -0.25, 0.25, 0.0, 0.25, -0.25, 0.0, -0.5, 0.0, 0.25, -0.25, -0.5, -0.25, -0.25, 0.25, -0.25, -0.5, -0.25, -0.25],
          [-0.5, -0.5, -0.75, -3.25, -0.5, -0.5, -0.5, -0.75, -0.25, -0.5, -0.5, -0.75, 0.75, 0.25, 1.25, -0.75, -0.5, 1.75, -0.25, -0.5, -0.75, -0.75, 0.0, -2.0, -0.5],
          [-0.5, -0.25, -0.75, 0.25, -0.75, -0.5, -0.25, -0.75, -0.75, 0.25, -0.5, -0.25, -0.5, -3.0, 1.5, -0.5, -0.25, 0.5, -0.5, 0.75, -0.5, -0.75, -1.25, 3.0, -1.25],
          [-0.5, 0.0, -1.0, 0.75, -0.5, -0.5, 0.0, -0.25, 0.0, 0.25, -0.5, 0.0, -0.25, -2.75, 0.5, -0.5, -0.25, 0.0, -0.25, 0.5, -0.25, -1.75, 0.5, -2.5, 0.5],
          [-0.5, -0.5, 0.25, 0.25, -0.5, -0.5, 0.0, -0.5, -0.25, 0.5, -0.5, -0.5, 0.0, 0.25, 0.25, 0.0, 0.0, 0.25, -0.5, -2.0, 0.0, 0.0, 0.0, -0.75, -1.25],
          [-0.5, -0.5, -0.25, 0.5, 0.0, -0.5, 0.0, -0.25, -0.25, -0.25, -0.5, -0.25, -0.25, -1.75, -1.25, -0.25, 0.0, 0.25, -0.5, -0.5, -0.25, -0.25, -0.25, 0.5, 1.0],
          [0.0, -0.25, 0.0, 0.0, 0.25, 0.0, -0.25, -0.25, 0.25, -0.25, 0.0, -0.25, -0.25, 0.0, -0.25, -0.25, -0.25, -0.25, 0.25, -0.5, -0.25, 0.0, -0.25, 0.25, -0.25],
          [-1.0, -0.75, -1.75, -3.75, 1.25, -1.0, -0.25, 2.75, -1.25, -2.25, -1.0, -0.5, 0.5, -1.25, 1.5, -0.75, -0.25, -8.0, -1.0, -1.5, -0.75, -1.0, 2.75, 3.75, -1.5],
          [0.5, 0.75, 0.25, 0.25, -0.25, 0.5, -0.25, -0.25, -0.25, 0.0, 0.5, 0.5, 0.0, 0.25, 0.25, -0.5, -0.25, -0.25, -0.25, -1.0, -0.5, -0.5, -0.25, -0.25, -0.75],
          [-0.25, -0.25, -0.25, -0.5, 0.0, -0.25, -0.25, 0.25, -0.25, 0.25, -0.25, -0.25, -1.25, 0.25, -0.5, -0.25, -0.25, 0.25, -0.25, 0.0, -0.25, -0.25, 0.5, -0.25, 0.0],
          [0.0, 0.5, -1.75, 0.25, -0.75, 0.0, 0.25, -0.25, 0.25, -0.5, 0.0, 0.25, -0.5, -0.25, -0.75, -1.5, 0.0, 0.75, 0.0, 0.0, -1.0, -0.5, -0.25, -0.5, -0.25],
          [-0.5, -0.25, 0.25, -0.5, 0.25, -0.5, 0.0, 0.25, 0.0, 0.25, -0.5, 0.0, 0.5, 0.25, -0.75, -0.5, -0.25, -1.5, -0.5, 0.0, -0.25, -0.25, -1.0, -1.0, 0.0],
          [0.0, -0.25, -0.5, -0.25, 0.0, 0.0, -0.25, -1.0, -0.25, 0.0, 0.0, -2.0, -0.5, -0.5, -0.5, 0.0, -1.0, -0.25, -0.5, 0.75, 0.75, 0.5, 0.25, 1.0, -0.5],
          [0.25, -0.25, -0.5, -0.25, 0.0, 0.25, 0.0, 0.25, 0.0, 0.0, 0.25, -0.5, -0.25, -0.5, -0.25, 0.25, 0.75, 0.0, 0.5, -0.5, -0.5, -0.5, -0.75, -0.25, -0.25],
          [-0.25, 1.25, 0.0, -0.25, -0.25, -0.25, -0.25, 0.25, -0.25, -0.25, -0.25, -0.5, 0.0, 0.0, 0.0, -0.25, -0.25, -0.25, -0.25, -0.25, -0.25, -0.25, 0.0, 0.25, 0.0],
          [0.25, -0.25, -1.0, -0.75, -0.25, 0.25, -0.75, -0.5, -0.5, -0.25, 0.0, 1.25, 0.0, -0.75, -0.5, 0.25, -0.5, 0.0, 0.0, -1.25, 0.25, 1.0, 0.5, -1.5, -0.25],
          [-0.5, -2.25, -0.5, -1.75, -2.25, -0.25, -0.25, -0.25, -0.25, -1.5, -0.25, 0.0, 0.5, 0.5, 0.25, -0.25, -0.25, 0.5, -0.25, 0.25, -0.5, -0.25, 0.0, 0.75, 0.0],
          [-0.75, -0.5, -0.5, 0.25, -0.5, -0.75, -0.5, -1.0, -0.5, 1.75, -0.75, -1.0, -3.25, 0.0, -0.25, -0.75, -0.5, 0.25, -1.25, -0.25, -0.75, -0.75, -0.25, 1.75, -0.5],
          [0.5, -0.75, 0.5, 0.5, -1.0, 0.5, -1.0, 0.75, -1.25, -0.25, 0.25, 0.5, -1.0, -1.0, -0.25, 0.75, -0.5, -0.75, -0.5, 0.0, 0.75, -1.75, -1.0, 1.0, -0.5],
          [-0.5, -0.5, -1.0, 1.75, -1.0, -0.25, -0.75, 0.5, -2.0, -1.0, -0.25, -0.5, -0.75, 1.5, -0.5, -0.5, -1.0, -4.5, 0.75, -0.25, -0.25, -0.5, 0.75, -2.25, 0.75]
        ],
      };
    },
    computed: {
      detectedLetters () {
        const ans = [];
        for (let i = 0; i < this.letterWeigths.length; i += 1) {
          let sum = 0;
          for (let j = 0; j < 25; j += 1) {
            sum += this.currentLetterMatrix[j] * this.letterWeigths[i][j];
          }
          if (sum > 0) ans.push(i);
        }
        return ans.map(lId => this.letters[lId]);
      }
    },
  }
</script>

<style lang="sass">

</style>