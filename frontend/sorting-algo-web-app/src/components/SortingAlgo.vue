<template>
  <div
    class="parent"
    style="
      position: fixed;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-image: linear-gradient(to right, #8e2de2, #4a00e0);
    "
  >
    <div class="d-grid gap-2 p-3 rounded" style="width: 200px">
      <button
        type="button"
        class="btn bg-dark text-white p-2 btn-outline-dark"
        data-bs-toggle="collapse"
        data-bs-target="#multiCollapseExample1"
        aria-expanded="false"
        aria-controls="multiCollapseExample1"
      >
        SortAlgoVi
      </button>
    </div>
    <div class="collapse rounded overlay" id="multiCollapseExample1" style="width: 200px">
      <div class="d-grid gap-2 p-2 rounded transparent">
        <div class="d-grid gap-1 p-1 transparent rounded">
          <div class="btn bg-dark text-white btn-static">
            <label @click="generateNewArray" class="h6"> Choose length </label>
          </div>
          <input
            type="range"
            class="form-range range-cust"
            id="customRange1"
            v-model="arrayLength"
            min="2"
            max="180"
          />
        </div>

        <div class="d-grid gap-1 transparent p-1 rounded">
          <div class="btn bg-dark text-white btn-static" disabled>
            <label class="h6 d-inline-block">Choose Speed</label>
          </div>
          <input
            type="range"
            class="form-range range-cust"
            id="customRange1"
            v-model="speed"
            min="1"
            max="200"
          />
        </div>

        <div class="d-grid gap-1 transparent p-1 rounded">
          <div class="btn bg-dark text-white">
            <lable class="h6">Choose algorithm</lable>
          </div>
          <select
            class="form-select bg-dark text-white btn-outline-dark"
            aria-label="Default select example"
            v-model="selected"
          >
            <option>BubbleSort</option>
            <option>SelectionSort</option>
            <option>QuickSort</option>
          </select>
        </div>

        <div class="d-grid gap-2 transparent p-1 rounded">
          <button @click="sortingAlgorithm" class="btn btn-dark p-2">Sort</button>
        </div>

        <div class="d-grid gap-2 transparent p-1 rounded">
          <button @click="stop" type="button" class="btn btn-dark p-2">Reset</button>
        </div>
      </div>
    </div>

    <div
      class="strip"
      v-for="i in parseInt(arrayLength)"
      :key="i"
      :style="getStripStyle(i)"
    ></div>
  </div>
</template>

<script>
import "bootstrap";
import "bootstrap/dist/css/bootstrap-grid.min.css";
import "bootstrap/dist/js/bootstrap.min.js";

export default {
  data() {
    return {
      arrayLength: "120",
      selected: "",
      array: [...Array(120)].map(() => [Math.ceil(Math.random() * 99), "black"]),
      speed: "1",
      isInSortingProcess: false,
      sorted: false,
      timeoutId: null,
      windowHeight: window.innerHeight,
    };
  },

  watch: {
    arrayLength: function (val) {
      this.array = [...Array(parseInt(val))].map(() => [
        Math.ceil(Math.ceil(Math.random() * 10000)),
        "black",
      ]);
      this.fillArray();
      this.isInSortingProcess = false;
      this.sorted = false;
    },
    speed: function (val) {
      this.speed = val;
    },
  },

  computed: {},

  methods: {
    sortingAlgorithm() {
      //is Array already sorted check
      console.log("FFFF" + this.sorted);
      if (this.sorted) {
        alert("The array is already sorted!");
        return;
      }
      switch (this.selected) {
        case "BubbleSort":
          this.bubbleSort();
          break;
        case "2":
          // selectionSort
          break;
      }
    },

    async bubbleSort() {
      console.log("gggg");
      // get in sorting process
      this.isInSortingProcess = true;
      let n;
      let swap;
      n = this.array.length - 1;

      do {
        swap = false;
        for (let i = 0; i < n; i++) {
          // if not in sorting process, terminate
          if (!this.isInSortingProcess) {
            return;
          }

          this.array[i][1] = "lightcoral";
          this.array[i + 1][1] = "orange";
          if (this.array[i][0] > this.array[i + 1][0]) {
            const temp = this.array[i][0];

            this.array[i][0] = this.array[i + 1][0];
            await this.sleep(this.speed);
            this.$forceUpdate();

            this.array[i + 1][0] = temp;
            await this.sleep(this.speed);
            this.$forceUpdate();
            swap = true;
          }
          this.array[i][1] = "purple";
          this.array[i + 1][1] = "purple";
        }
        this.array[n][1] = "lightgreen";
        n--;
      } while (swap);

      // color the rest green, end sorting process
      for (let i = 0; i <= n; i++) {
        this.array[i][1] = "lightgreen";
      }
      this.isInSortingProcess = false;
      this.sorted = true;
      return;
    },

    fillArray() {
      this.array = [...Array(parseInt(this.arrayLength))].map(() => [
        Math.ceil(Math.ceil(Math.random() * 99)),
        "black",
      ]);
      this.isInSortingProcess = false;
      this.sorted = false;
    },
    generateNewArray() {
      this.array = [...Array(parseInt(this.arrayLength))].map(() => [
        Math.ceil(Math.ceil(Math.random() * 99)),
        "black",
      ]);
      this.isInSortingProcess = false;
      this.sorted = false;
    },
    getRndInteger(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },
    created() {
      this.fillArray();
    },
    stop() {
      if (this.timeoutId != null) {
        clearTimeout(this.timeoutId);
        this.created();
        this.timeoutId = null;
      } else {
        alert("is reset");
      }
    },
    sleep() {
      return new Promise((resolve) => {
        this.timeoutId = setTimeout(resolve, this.speed);
      });
    },

    // animation
    getStripStyle(index) {
      const i = index - 1;
      const sWidth = 100 / parseInt(this.arrayLength);
      return {
        width: sWidth * 0.6 + "%",
        marginLeft: sWidth * 0.2 + "%",
        marginRight: sWidth * 0.2 + "%",
        height: this.windowHeight * 0.6 * (this.array[i][0] / 100) + "px",
        backgroundColor: this.array[i][1],
      };
    },
    onResize() {
      this.windowHeight = window.innerHeight;
    },
  },
  mounted() {
    this.$nextTick(() => {
      window.addEventListener("resize", this.onResize);
    });
  },
};

// function swapArr(arr, a, b) {
//   const temp = arr[a];
//   arr[a] = arr[b];
//   arr[b] = temp;
// }
</script>

<style scoped>
/* scrollbars */
.range-cust::-webkit-slider-thumb {
  background: black;
}
.range-cust::-moz-range-thumb {
  background: black;
}

.range-cust::-ms-thumb {
  background: black;
}

.bar-container {
  flex-direction: row;
}

.container {
  position: fixed;
  left: 100px;
}

.bar {
  width: 4px;
  background-color: black;
  display: inline-block;
  margin: 0 1px;
  position: relative;
  top: -10px;
}
.strip {
  display: inline-block;
  position: relative;
  top: 150px;
}

.parent {
  position: relative;
}

.overlay {
  position: fixed;
  z-index: 1;
}

.transparent:before {
  opacity: 0.6;
  /* background-color: red; */
  pointer-events: none;
}
</style>
