<template>
  <div class="flex" style="overflow: hidden">
    <nav
      class="navbar bg-dark navbar-expand-lg bg-body-tertiary navbar-dark"
      data-bs-theme="dark"
    >
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Sorting Algorithm Visualizer</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav"></ul>
        </div>
      </div>
    </nav>
    <div style="overflow: hidden">
      <SortingAlgo />
    </div>
  </div>
</template>

<script>
import SortingAlgo from "./components/SortingAlgo.vue";

export default {
  components: {
    SortingAlgo,
  },
  methods: {},
};
</script>

<style></style>
