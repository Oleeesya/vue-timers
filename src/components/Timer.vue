<template>
  <div class="wrapper">
    <div class="wrapper__element">
      <div class="timer" :class="{ time_off: timer.isPause }">
        <div class="timer__block">
          <div
            class="time time-hour"
            v-bind:class="{ time_off: timer.isPause }"
          >
            {{ timer.hourElement }}
          </div>
        </div>
        <div class="timer__block">
          <div
            class="time time-minute"
            v-bind:class="{ time_off: timer.isPause }"
          >
            {{ timer.minuteElement }}
          </div>
        </div>
        <div class="timer__block">
          <div
            class="time time-second"
            v-bind:class="{ time_off: timer.isPause }"
          >
            {{ timer.secondElement }}
          </div>
        </div>
      </div>
      <div class="buttons">
        <div
          class="button"
          :class="{ button_hidden: timer.isStart }"
          @click="startButton(timer)"
        >
          <svg
            :class="{ button_off: timer.isStart }"
            width="17"
            height="20"
            viewBox="0 0 17 20"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path d="M0 20V0L17 10L0 20Z" fill="#9E9E9E" />
            {{ timer.start }}
          </svg>
        </div>
        <div
          class="button"
          :class="{ button_hidden: timer.isPause }"
          @click="pauseButton(timer)"
        >
          <svg
            width="10"
            height="20"
            viewBox="0 0 10 20"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <rect
              :class="{ button_off: timer.isStart }"
              x="7"
              width="3"
              height="20"
              fill="#9E9E9E"
            />
            <rect
              :class="{ button_off: timer.isStart }"
              width="3"
              height="20"
              fill="#9E9E9E"
            />
            {{ timer.pause }}
          </svg>
        </div>
        <div
          class="button"
          :class="{ button_off: timer.isPause }"
          @click="stopButton(timer)"
        >
          <svg
            width="20"
            height="20"
            viewBox="0 0 20 20"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <rect
              :class="{ button_off: timer.isStart }"
              width="20"
              height="20"
              fill="#9E9E9E"
            />
            {{ timer.stop }}
          </svg>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Timer",
  props: {
    onAdd: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      timer: {
        hour: 0,
        minute: 0,
        second: 0,
        secondElement: "",
        minuteElement: "",
        hourElement: "",

        interval: "",
        start: "",
        pause: "",
        stop: "",
        isStart: false,
        isPause: true,
        id: null,
      },
    };
  },
  methods: {
    startButton() {
      clearInterval(this.timer.interval);
      this.timer.interval = setInterval(() => {
        this.startTimer();
      }, 1000);
      this.timer.isStart = true;
      this.timer.isPause = false;
    },
    startTimer() {
      this.timer.second++;
      if (this.timer.second < 9) {
        this.timer.secondElement = "0" + this.timer.second;
      }
      if (this.timer.second > 9) {
        this.timer.secondElement = this.timer.second;
      }
      if (this.timer.second > 59) {
        this.timer.minute++;
        this.timer.minuteElement = "0" + this.timer.minute + ":";
        this.timer.second = 0;
        this.timer.secondElement = "0" + this.timer.second;
      }
      if (this.timer.minute < 9 && this.timer.minute > 0) {
        this.timer.minuteElement = "0" + this.timer.minute + ":";
      }
      if (this.timer.minute > 9) {
        this.timer.minuteElement = this.timer.minute + ":";
      }
      if (this.timer.minute > 59) {
        this.timer.hour++;
        this.timer.hourElement = "0" + this.timer.hour + ":";
        this.timer.minute = 0;
        this.timer.minuteElement = "0" + this.timer.minute + ":";
      }
      if (this.timer.hour < 9 && this.timer.hour > 0) {
        this.timer.hourElement = "0" + this.timer.hour + ":";
      }
      if (this.timer.hour > 9) {
        this.timer.hourElement = this.timer.hour + ":";
      }
    },
    pauseButton() {
      clearInterval(this.timer.interval);
      this.timer.isStart = false;
      this.timer.isPause = true;
    },
    clearFields() {
      this.timer.hour = 0;
      this.timer.minute = 0;
      this.timer.second = 0;
      this.timer.hourElement = "";
      this.timer.minuteElement = "";
      this.timer.secondElement = "";
    },
    stopButton() {
      clearInterval(this.timer.interval);
      this.clearFields(this.timer);
      this.timer.isStart = false;
      this.timer.isPause = true;
    },

    newTimer() {
      let timer = {
        hour: this.hour,
        minute: this.minute,
        second: this.second,
        secondElement: this.secondElement,
        minuteElement: this.minuteElement,
        hourElement: this.hourElement,

        interval: this.interval,
        start: this.start,
        pause: this.pause,
        stop: this.stop,
        isStart: this.isStart,
        isPause: this.isPause,
        id: Date.now().toString(),
      };
      let newTimer = Object.assign(timer);
      newTimer.id = Date.now().toString();

      return newTimer;
    },
  },
};
</script>

<style>
.wrapper {
  display: contents;
}

.wrapper__element {
  max-width: 225px;
  background: #696969;
}

.timer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 18px;
  padding: 21px 0 20px;
  border-bottom: 1px solid;
  color: #fff;
}

.time {
  font-family: "GothamPro", Arial, sans-serif;
  color: #fff;
  font-size: 22px;
  font-weight: 400;
  line-height: 21.05px;
}

.time_off {
  color: #9e9e9e;
}

.buttons {
  position: relative;
  display: flex;
  justify-content: space-between;
  padding: 20px 0 20px;
  max-width: 85px;
  min-width: 85px;
  margin: 0 auto;
}

.button {
  fill: #fff;
  height: 20px;
}

.button_hidden {
  display: none;
}

.button_off {
  fill: #fff;
}

.add {
  background: #696969;
  display: flex;
  justify-content: center;
  align-items: center;
  background-size: 20px;
  height: 120px;
}
</style>
