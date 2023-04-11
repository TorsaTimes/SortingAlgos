<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-4 box1 min-vh-100 bg-success p-2 gap-2 bg-dark">
        <div class="d-grid gap-2">
          <div class="d-grid gap-2 bg-secondary p-2 rounded">
            <div class="btn bg-dark text-white btn-static" disabled>
              <label>Choose array length</label>
            </div>
            <!-- <label for="customRange1" class="form-label">Example range</label> -->
            <input
              type="range"
              class="form-range range-cust"
              id="customRange1"
              v-model="value"
              min="2"
              max="180"
              @click="fillArray()"
            />
          </div>

          <div class="d-grid gap-2 bg-secondary p-2 rounded">
            <div class="btn bg-dark text-white btn-static" disabled>
              <label>Choose sorting Speed</label>
            </div>
            <!-- <label for="customRange1" class="form-label">Example range</label> -->
            <input
              type="range"
              class="form-range range-cust"
              id="customRange1"
              v-model="speed"
              min="1"
              max="120"
            />
          </div>

          <div class="d-grid gap-2 bg-secondary p-2 rounded">
            <div class="btn bg-dark text-white">
              <label>Choose sorting algorithm</label>
            </div>
            <select
              class="form-select bg-dark text-white"
              aria-label="Default select example"
              v-model="selected"
            >
              <option value="1">BubbleSort</option>
              <option value="2">MergeSort</option>
              <option value="3">QuickSort</option>
            </select>
          </div>

          <div class="d-grid gap-2 bg-secondary p-2 rounded">
            <!-- <div class="btn bg-dark text-white">
                        <label>Choose array length</label>
                      </div> -->
            <button @click="sortingAlgorithm()" type="button" class="btn btn-dark p-2">
              Sort
            </button>
            <!-- <button @click="fillArray()" type="button" class="btn btn-dark p-2">
              Shuffle
            </button> -->
          </div>

          <!-- <div class="mt-2 text-primary">Value1: {{ value }}</div>
          <div class="mt-2 text-primary">Drop2: {{ selected }}</div>
          <div class="mt-2 text-primary">Speed3: {{ speed }}</div> -->
        </div>
      </div>
      <div class="col-8 box1 min-vh-100 bg-secondary">
        <div
          class="bar-container"
          style="height: 550px; overflow: hidden; padding-top: 20px; bottom: 30px"
        >
          <div
            class="bar"
            v-for="(number, index) in array"
            :key="index"
            :style="{
              height: number + 'px',
              bottom: '0',
              backgroundColor: index === sortedIndex ? 'red' : 'black',
              width: 'calc(100% / ' + array.length + ' - 2px)',
            }"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  value: "0",
  selected: null,
  array: [],
  data() {
    return {
      value: "0",
      selected: "",
      array: [],
      sortedIndex: null,
      speed: 0,
    };
  },
  methods: {
    async bubbleSort() {
      let len = this.array.length;
      let checked;
      do {
        checked = false;
        for (let i = 0; i < len; i++) {
          if (this.array[i] > this.array[i + 1]) {
            let tmp = this.array[i];
            this.array[i] = this.array[i + 1];
            this.array[i + 1] = tmp;
            this.sortedIndex = i + 1;
            await this.sleep();
            checked = true;
          }
        }
      } while (checked);
      this.sortedIndex = null;
    },
    sleep() {
      return new Promise((resolve) => setTimeout(resolve, this.speed));
    },
    fillArray() {
      this.array = [];
      for (let i = 0; i < this.value; i++) {
        this.array.push(this.getRndInteger(10, 500));
      }
    },
    getRndInteger(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },
    created() {
      this.fillArray();
      console.log(this.array);
    },
    sortingAlgorithm() {
      switch (this.selected) {
        case "1":
          console.log("los");
          this.bubbleSort();
          break;
        case "2":
          break;
        default:
        // code block
      }
    },
  },
  computed: {
    barWidth() {
      // Berechnung der Breite der Balken basierend auf der Länge des Arrays
      const maxArrayLength = 180; // Maximale Breite
      const maxBarWidth = 30; // Maximale Balkenbreite
      return (number) => {
        const barWidth = (number / maxArrayLength) * maxBarWidth;
        return barWidth;
      };
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.range-cust::-webkit-slider-thumb {
  background: black;
}
.range-cust::-moz-range-thumb {
  background: black;
}

.range-cust::-ms-thumb {
  background: black;
}

/* button lable
.btn-static {
  background-color: black;
  border: 1px solid black;
  cursor: default;
}
.btn-static:active {
  -moz-box-shadow:    inset 0 0 0px black;
  -webkit-box-shadow: inset 0 0 0px black;
  box-shadow:         inset 0 0 0px black;
} */

.bar-container {
  /* display: flex; */
  /* margin-top: -10px; */
  flex-direction: row;
}

.container {
  position: fixed;
  left: 100px;
}

.bar {
  width: 5px;
  background-color: black;
  display: inline-block;
  margin: 0 1px;
  position: relative;
  top: -10px; /* oder den gewünschten Wert anpassen */
}
</style>
