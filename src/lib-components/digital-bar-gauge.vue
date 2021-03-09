<script>
export default /*#__PURE__*/ {
  name: "DigitalBarGauge", // vue component name
  props: ["maxData", "minData", "data", "symbol", "normalColor"],
  data() {
    return {
      maxValue: this.maxData,
      minValue: this.minData,
      barValue: this.data,
      symbolValue: this.symbol,
      screenValue: 0,
      barStatus: "",
      bar: "",
      average: 0,
    };
  },
  watch: {
    data: function (val) {
      return (this.barValue = val);
    },
  },
  computed: {
    barStatusColor() {
      this.barStatus = this.normalColor;
      return this.barStatus;
    },
    barHeight() {
      this.average = this.maxValue + Math.abs(this.minValue);
      this.average = (200 * Math.abs(this.minValue)) / this.maxValue;
      if (this.barValue >= 0) {
        if (this.barValue >= this.maxValue) {
          this.screenValue = 200;
        } else if (this.barValue < this.minValue) {
          this.screenValue = 0;
        } else {
          this.screenValue =
            (this.barValue * (200 - this.average)) / this.maxValue +
            this.average;
        }
      } else if (this.barValue < 0) {
        this.average = this.maxValue + Math.abs(this.minValue);
        this.average = (200 * Math.abs(this.minValue)) / this.maxValue;
        this.screenValue = (this.barValue * 200) / this.maxValue + this.average;
      }
      return this.screenValue;
    },
  },
};
</script>

<template>
  <div class="background">
    <div class="max">{{ maxValue }}</div>
    <div class="min">{{ minValue }}</div>
    <div class="inside">
      <div
        class="bar"
        :style="{
          height: barHeight + 'px',
          backgroundColor: barStatusColor,
          boxShadow: '0 0 10px ' + barStatusColor,
        }"
      ></div>
    </div>
    <div class="value">{{ barValue }} {{ symbolValue }}</div>
  </div>
</template>

<style scoped>
.background {
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif !important;
  position: relative !important;
  width: 130px !important;
  height: 270px !important;
  background-color: rgb(145, 145, 145) !important;
  border-radius: 10px !important;
  box-shadow: 0 0 15px grey !important;
  user-select: none !important;
}

.max {
  position: absolute !important;
  width: 100% !important;
  top: 22px !important;
  right: 6px !important;
  font-size: 14px !important;
  text-align: right !important;
}

.min {
  position: absolute !important;
  width: 100% !important;
  bottom: 40px !important;
  right: 6px !important;
  font-size: 14px !important;
  text-align: right !important;
}

.inside {
  position: absolute !important;
  width: 55px !important;
  height: 200px !important;
  top: 25px !important;
  left: 36px !important;
  background-color: grey !important;
  border: 2px solid rgb(64, 64, 64) !important;
  border-radius: 3px !important;
  display: flex !important;
  align-items: center !important;
  text-align: center !important;
  box-shadow: inset 0 0 10px rgb(52, 52, 52) !important;
}

.bar {
  position: absolute !important;
  bottom: 0 !important;
  width: 55px !important;
  transition: 0.3s !important;
}

.value {
  position: absolute !important;
  bottom: 13px !important;
  width: 100% !important;
  height: 100% !important;
  color: white !important;
  font-size: 20px !important;
  display: flex !important;
  justify-content: center !important;
  align-items: flex-end !important;
  text-align: center !important;
  z-index: 99999 !important;
}
</style>
