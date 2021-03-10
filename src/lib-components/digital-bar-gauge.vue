<script>
export default /*#__PURE__*/ {
  name: "DigitalBarGauge", // vue component name
  props: {
    maxData: {
      default: false,
    },
    minData: {
      default: false,
    },
    data: {
      default: false,
    },
    symbol: {
      type: String,
      default: false,
    },
    normalColor: {
      type: String,
      default: false,
    },
    date: {
      type: String,
      default: false,
    },
  },
  data() {
    return {
      maxValue: this.maxData,
      minValue: this.minData,
      barValue: this.data,
      symbolValue: this.symbol,
      time: this.date,
      screenValue: 0,
      barStatus: "",
      average: 0,
      negative: 0,
    };
  },
  watch: {
    data: function (val) {
      return (this.barValue = val);
    },
    date: function (val) {
      return (this.time = val);
    },
  },
  computed: {
    barStatusColor() {
      this.barStatus = this.normalColor;
      return this.barStatus;
    },
    barHeight() {
      if (this.minValue < 0) {
        if (this.barValue <= this.minValue) {
          this.screenValue = 0;
          return this.screenValue;
        } else if (this.barValue > this.maxValue) {
          this.screenValue = 200;
          return this.screenValue;
        } else if (this.barValue < 0) {
          this.average = Math.abs(this.barValue) + this.minValue;
          this.screenValue =
            (200 * Math.abs(this.average)) /
            (this.maxValue + Math.abs(this.minValue));
        } else if (this.barValue >= 0) {
          this.average =
            (200 * Math.abs(this.minValue)) /
            (this.maxValue + Math.abs(this.minValue));
          this.negative =
            (200 * this.barValue) / (this.maxValue + Math.abs(this.minValue));
          this.screenValue = this.average + this.negative;
        }
      } else if (this.minValue >= 0) {
        if (this.barValue <= this.minValue) {
          this.screenValue = 0;
          return this.screenValue;
        } else if (this.barValue > this.maxValue) {
          this.screenValue = 200;
          return this.screenValue;
        } else if (this.barValue > this.minValue) {
          this.average = (200 * this.minValue) / (this.maxValue - this.minValue)
          this.negative = (200 * this.barValue) / (this.maxValue - this.minValue)
          this.screenValue = this.negative - this.average;
        }
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
    <div class="value">{{ barValue.toFixed(1) }} {{ symbolValue }}</div>
    <div class="date">{{ time }}</div>
  </div>
</template>

<style scoped>
.background {
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif !important;
  position: relative !important;
  width: 130px !important;
  height: 280px !important;
  background-color: rgb(145, 145, 145) !important;
  border-radius: 10px !important;
  box-shadow: 0 0 15px grey !important;
  user-select: none !important;
}

.max {
  position: absolute !important;
  width: 100% !important;
  top: 22px !important;
  right: 4px !important;
  font-size: 12px !important;
  text-align: right !important;
}

.min {
  position: absolute !important;
  width: 100% !important;
  bottom: 50px !important;
  right: 4px !important;
  font-size: 12px !important;
  text-align: right !important;
}

.inside {
  position: absolute !important;
  width: 45% !important;
  height: 71.3% !important;
  top: 25px !important;
  left: 25px !important;
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
  width: 100% !important;
  transition: 0.3s !important;
}

.value {
  position: absolute !important;
  bottom: 23px !important;
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

.date {
  position: absolute !important;
  bottom: 8px !important;
  width: 100% !important;
  height: 100% !important;
  color: white !important;
  font-size: 10px !important;
  display: flex !important;
  justify-content: center !important;
  align-items: flex-end !important;
  text-align: center !important;
  z-index: 99999 !important;
}
</style>
