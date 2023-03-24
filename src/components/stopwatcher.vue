<template>
  <div class="stopwatcher">
    <div class="stopwatcher__body">
      <div class="stopwatcher__body__time">
        <div class="stopwatcher__body__time__hours">{{ data.hours }} :</div>
        <div class="stopwatcher__body__time__minutes">{{ data.minutes }} :</div>
        <div class="stopwatcher__body__time__seconds">{{ data.seconds }} :</div>
        <div class="stopwatcher__body__time__miliseconds">{{ data.miliseconds }}</div>
      </div>
      <div class="stopwatcher__body__tools">
        <button class="stopwatcher__body__button_positive" @click="positiveFunction">
          {{ positiveButtonTitle }}
        </button>
        <button class="stopwatcher__body__button_negative" @click="reset">Reset</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from "vue";

const data = reactive({
  hours: "00",
  minutes: "00",
  seconds: "00",
  miliseconds: "000",
});

const positiveButtonTitle = ref("Start");

let positiveFunction = start;
let timeInteval;

let startTime = 0;
let delay = 0;

function start() {
  startTime = Date.now();
  timeInteval = setInterval(watchTime, 10);
  positiveFunction = stop;
  positiveButtonTitle.value = "Stop";
}

function stop() {
  clearInterval(timeInteval);
  const stopTime = Date.now();
  positiveFunction = () => resume(stopTime);
  positiveButtonTitle.value = "Resume";
}

function resume(stopTime) {
  const resumeTime = Date.now();
  delay += resumeTime - stopTime;
  timeInteval = setInterval(watchTime, 10);
  positiveFunction = stop;
  positiveButtonTitle.value = "Stop";
}

function reset() {
  startTime = delay = 0;
  data.hours = data.minutes = data.seconds = "00";
  data.miliseconds = "000";
  clearInterval(timeInteval);
  positiveFunction = start;
  positiveButtonTitle.value = "Start";
}

const watchTime = () => {
  const current = Date.now();
  const timeCount = current - startTime - delay;
  data.miliseconds = (timeCount % 1000).toString().padStart(3, "0");

  const t = new Date(timeCount);
  const time = new Date(t.setHours(0));

  data.seconds = time.getSeconds().toString().padStart(2, "0");
  data.minutes = time.getMinutes().toString().padStart(2, "0");
  data.hours = time.getHours().toString().padStart(2, "0");
};
</script>

<style lang="scss" scoped>
@import "../assets/mixins/button.scss";
.stopwatcher {
  padding: 30px;
  border: 1px solid #000;
  border-radius: 20px;
  background: #000;
  color: #86fd41;
  margin: 15px;
}
.stopwatcher__body__time {
  font-size: 5em;
  margin-bottom: 15px;
  font-family: "Typo Digit";
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}
.stopwatcher__body__tools {
  display: flex;
  justify-content: center;
  gap: 10px;
}

.stopwatcher__body__button_positive {
  @include button(#fff);
  background: #000000;
}
.stopwatcher__body__button_negative {
  @include button(#f31e1e);
  background: #000000;
}

@media (max-width: 551px) {
  .stopwatcher__body__time {
    font-size: 4em;
  }
}
@media (max-width: 450px) {
  .stopwatcher__body__time {
    font-size: 2em;
  }
}
</style>
