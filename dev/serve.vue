<script>
import Vue from "vue";
// Uncomment import and local "components" registration if library is not registered globally.
import { DigitalBarGauge } from "@/entry.esm";

export default Vue.extend({
  name: "ServeDev",
  components: {
    DigitalBarGauge,
  },
  data() {
    return {
      data: 0,
      array: [
        { id: 1, data: 560, max: 600, min: 0, symbol: "°C" },
        { id: 2, data: 615, max: 700, min: -100, symbol: "°F" },
        { id: 3, data: 350.2, max: 800.2, min: -300, symbol: "V" },
        { id: 4, data: 700, max: 800, min: -300, symbol: "M" },
      ],
    };
  },
  created() {
    this.refresh();
  },
  methods: {
    refresh() {
      setInterval(() => {
        this.array.forEach((element) => {
          element.data = element.data - 25.3;
        });
      }, 1000);
    },
  },
});
</script>

<template>
  <div id="app">
    <div style="display: flex">
      <div v-for="(value, index) in array" :key="index">
        <digital-bar-gauge
          style="margin-right: 20px"
          :maxData="value.max"
          :minData="value.min"
          :data="value.data"
          :symbol="value.symbol"
          normalColor="rgb(79, 255, 70)"
          date="12/11/2021 16:20:40"
        />
      </div>
    </div>
    <!--
          <digital-bar-gauge
          :maxData="200"
          :minData="-10.5"
          :data="100"
          symbol="°F"
          normalColor="rgb(79, 255, 70)"
          date="12/11/2021 16:20:40"
        />
        -->
  </div>
</template>
