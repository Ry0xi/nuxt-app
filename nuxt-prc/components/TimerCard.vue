<template>
  <div class="timer-card">
    <div class="remain-time">{{formatTime}}</div>
    <input type="number" class="input-set-time no-spin" placeholder="3000 -> 30:00" :disabled="timerOn">
    <div class="btn-start" v-if="!timerOn" @click="startTimer"></div>
    <div class="btn-stop" v-if="timerOn" @click="stopTimer"></div>
    <div class="status-lamp" :class="timerOn ? 'is-active' : ''"></div>
  </div>
</template>

<script>
export default {
  name: "TimerCard",
  data() {
    return {
      min: 60,
      sec: 0,
      passedTime: 0,
      startedTime: null,
      objTimer: null,
      timerOn: false
    }
  },
  methods: {
    startTimer() {
      this.startedTime = Date.now()
      this.timerOn = true

      // setInterval内でのthisはコンポーネントインスタンスを示さない
      let self = this
      this.objTimer = setInterval(function() {
        self.count()
      }
      , 1000)
    },
    stopTimer() {
      clearInterval(this.objTimer)
      this.timerOn = false
    },
    count() {
      this.passedTime++

      if (this.sec <= 0 && this.min >= 1) {
        this.min--
        this.sec = 59
      } else if (this.sec <= 0 && this.min <= 0) {
        this.complete()
      } else {
        this.sec--
      }
    },
    complete() {
      clearInterval(this.objTimer)
    }
  },
  computed: {
    formatTime() {
      let timeString = ""
      let sMin = String(this.min)
      let sSec = String(this.sec)
      if (sMin.length < 2) {
        sMin = "0" + sMin
      }
      if (sSec.length < 2) {
        sSec = "0" + sSec
      }
      timeString = sMin + " : " + sSec
      return timeString
    },
    validateTime() {
      if (this.min < 0) {
        this.min = 0
      }
      if (this.min > 999) {
        this.min = 999
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.timer-card {
  position: relative;

  width: 200px;
  height: 100px;
  border-radius: 16px;
  background-color: #1E1E1E;
  font-size: 16px;
}

.timer-card:hover {
  .remain-time {
    top: 0;
    left: 8px;
    transform: translate(0, 0);
  }

  .btn-start {
    visibility: visible;
    opacity: 1;
  }

  .btn-stop {
    visibility: visible;
    opacity: 1;
  }

  .input-set-time {
    visibility: visible;
    opacity: 1;
  }
}

.remain-time {
  position: absolute;
  top: 50%;
  left: 50%;
  width: max-content;
  transform: translate(-50%, -50%);
  font-size: 3em;
  user-select: none;
  
  transition: all 0.1s linear;
}

.btn-start,
.btn-stop {
  width: 26px;
  height: 26px;
  border: 2px solid #ccc;
  border-radius: 50%;
  position: absolute;
  bottom: 7px;
  right: 36px;
  cursor: pointer;

  visibility: hidden;
  opacity: 0;
  transition: all 0.2s linear;
}

.btn-start {
  &::after {
    content: "";
    display: block;
    width: 9px;
    height: 12px;
    clip-path: polygon(0 0, 0% 100%, 100% 50%);
    background-color: #ccc;
    position: absolute;
    top: 50%;
    left: 54%;
    transform: translate(-50%, -50%);
  }
}

.btn-stop {
  &::after {
    content: "";
    display: block;
    width: 8px;
    height: 12px;
    border-left: 3px solid #ccc;
    border-right: 3px solid #ccc;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}

.input-set-time {
  display: block;
  visibility: hidden;
  opacity: 0;
  position: absolute;
  left: 10px;
  bottom: 8px;
  width: 7.4em;
  height: 1.5em;
  padding: 0 0.6em;
  background-color: #aaa;
  border-radius: 4px;
  border: 1px solid #ddd;
  transition: all 0.2s linear;

  &:disabled {
    opacity: 0.4 !important;
  }
}

.status-lamp {
  position: absolute;
  right: 12px;
  bottom: 12px;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  --status-color: red;
  background-color: var(--status-color, gray);
  box-shadow: 0 0 8px var(--status-color, gray);
}

.status-lamp.is-active {
  --status-color: lightgreen;
}


.no-spin::-webkit-inner-spin-button,
.no-spin::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
    -moz-appearance:textfield;
}
</style>