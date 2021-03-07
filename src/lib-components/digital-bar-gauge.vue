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
        } else if (this.barValue <= this.minValue) {
          this.screenValue = this.average;
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
  methods: {
    start() {
      this.bar = document.querySelector("div.bar");
      this.average = this.maxValue + Math.abs(this.minValue);
      this.average = (200 * Math.abs(this.minValue)) / this.maxValue;
      if (this.barValue >= 0) {
        if (this.barValue >= this.maxValue) {
          this.screenValue = 200;
          this.bar.style.height = this.screenValue + "px";
        } else if (this.barValue <= this.minValue) {
          this.screenValue = this.average;
          this.bar.style.height = this.screenValue + "px";
        } else {
          this.screenValue =
            (this.barValue * (200 - this.average)) / this.maxValue +
            this.average;
          this.bar.style.height = this.screenValue + "px";
        }
      } else if (this.barValue < 0) {
        this.average = this.maxValue + Math.abs(this.minValue);
        this.average = (200 * Math.abs(this.minValue)) / this.maxValue;
        this.screenValue = (this.barValue * 200) / this.maxValue + this.average;
        this.bar.style.height = this.screenValue + "px";
      }
    },
  },
  mounted() {
    this.start();
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
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  position: relative;
  width: 130px;
  height: 270px;
  background-color: rgb(145, 145, 145);
  border-radius: 10px;
  box-shadow: 0 0 15px grey;
  user-select: none;
}

.max {
  position: absolute;
  width: 100%;
  top: 22px;
  right: 6px;
  font-size: 14px;
  text-align: right;
}

.min {
  position: absolute;
  width: 100%;
  bottom: 40px;
  right: 6px;
  font-size: 14px;
  text-align: right;
}

.inside {
  position: absolute;
  width: 55px;
  height: 200px;
  top: 25px;
  left: 36px;
  background-color: grey;
  border: 2px solid rgb(64, 64, 64);
  border-radius: 3px;
  display: flex;
  align-items: center;
  text-align: center;
  box-shadow: inset 0 0 10px rgb(52, 52, 52);
}

.bar {
  position: absolute;
  bottom: 0;
  width: 55px;
  transition: 0.3s;
}

.value {
  position: absolute;
  bottom: 13px;
  width: 100%;
  height: 100%;
  color: white;
  font-size: 20px;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  text-align: center;
  z-index: 99999999999999999999999999;
}
</style>
