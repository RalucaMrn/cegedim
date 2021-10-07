<template>
  <div class="container">
    <p>
      Introduceti un sir de numere cuprinse intre 100 si 999 separate prin
      virgula:
    </p>
    <div class="input-box">
      <input type="text" v-model="numbers" class="numbers-input" />
      <button @click="calculate" :disabled="isDisabled">Calculeaza</button>
    </div>

    <p v-show="showNumbersMessage" class="text-warning bg-dark">
      Te rog sa verifici daca ai inclus doar numere!
    </p>

    <p v-show="showValuesMessage" class="text-warning bg-dark">
      Te rog sa verifici daca ai inclus doar numere peste 100!
    </p>

    <p v-if="finalResult.length > 0">
      Maximele din clase: <span>{{ finalResult }}</span>
    </p>
    <p v-if="moduloResult.length > 0">Modulo: {{ moduloResult }}</p>

    <div class="grid-wrapper mt-5">
      <div v-for="(box, index) in quizArray" :key="index">
        <div class="box" @click="showLetter(index)">
          <div v-if="disclosedBoxes.includes(index)">
            {{ box ? box : '&#11093;'}}
          </div>
          <div v-else>?</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "NumbersApp",
  data() {
    return {
      numbers: "",
      showNumbersMessage: false,
      showValuesMessage: false,
      finalResult: [],
      moduloResult: [],
      quizArray: [],
      disclosedBoxes: []
    };
  },

  computed: {
    sortedNumbersArray() {
      return this.numbers
        .replace(/\s+/g, "")
        .split(",")
        .map(Number)
        .sort(function (a, b) {
          return a - b;
        });
    },

    isDisabled() {
      return this.numbers.length === 0;
    },
  },

  methods: {
    calculate() {
      this.showNumbersMessage = false;
      this.showValuesMessage - false;
      this.finalResult = [];
      this.moduloResult = [];
      if (this.sortedNumbersArray.includes(NaN)) {
        this.showNumbersMessage = true;
        return;
      }

      let valueCheck = this.sortedNumbersArray.every((x) => x >= 100);
      if (!valueCheck) {
        this.showValuesMessage = true;
        return;
      }

      let ranges = [
        "100-199",
        "200-299",
        "300-399",
        "400-499",
        "500-599",
        "600-699",
        "700-799",
        "800-899",
        "900-999",
      ];

      let results = this.checkRange(ranges, this.sortedNumbersArray);
      results.forEach((result) => {
        if (result.values) {
          let biggestValue = result.values.slice(-1)[0];

          this.finalResult.push(biggestValue);
        }
      });

      let alphabet = "abcdefghijklmnopqrstuvwxyz".toUpperCase();
      this.moduloResult = this.finalResult.map(
        (item) => alphabet.split("")[item % 26]
      );

      this.buildQuizBoard();
    },

    checkRange(ranges, values) {
      return ranges.map(function (range) {
        var ranges = range.split(/[-+]/);
        var min = +ranges[0];
        var max = +ranges[1] || Infinity;
        return values.reduce(function (acc, x) {
          if (x > min && x <= max) {
            acc.range = range;
            acc.values = (acc.values || []).concat(x);
          }
          return acc;
        }, {});
      });
    },

    buildQuizBoard() {
      this.quizArray = [];
      this.moduloResult.forEach((char) => {
        this.quizArray.push(char);
      });
      if (this.quizArray.length < 16) {
        for (let char = 0; char < 16 - this.moduloResult.length; char++) {
          this.quizArray.push(null);
        }
      }

      this.quizArray = this.shuffle(this.quizArray);
    },

    shuffle(array) {
      let currentIndex = array.length,
        randomIndex;

      while (currentIndex != 0) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;
        [array[currentIndex], array[randomIndex]] = [
          array[randomIndex],
          array[currentIndex],
        ];
      }
      return array;
    },

    showLetter(index) {
      this.disclosedBoxes.push(index);
    },
  }
};
</script>

<style scoped>
.grid-wrapper {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  grid-gap: 10px;
  background-color: #fff;
  color: #444;
}

.box {
  background-color: #444;
  color: #fff;
  border-radius: 5px;
  padding: 20px;
  font-size: 150%;
}

.input-box {
  margin-bottom: 16px;
}

input.numbers-input {
  margin-right: 10px;
  width: 350px;
}
</style>
