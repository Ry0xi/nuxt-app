<template>
  <div
    class="timer-card"
    :class="{isActive: timerOn, isPaused: timerPaused, isCompleted: timerCompleted}"
  >
    <div class="remain-time">{{formattedTime}}</div>
    <TimerInput
      class="input-set-time"
      v-model="inputTimeStr"
      :disabled="timerOn || timerPaused"
    />
    <div class="btn-operate-timer btn-start" v-if="!timerOn && !timerPaused && (min || sec)" @click="startTimer"></div>
    <div class="btn-operate-timer btn-continue" v-if="!timerOn && timerPaused" @click="continueTimer"></div>
    <div class="btn-operate-timer btn-pause" v-if="timerOn && !timerPaused && !timerCompleted" @click="pauseTimer"></div>
    <div class="btn-operate-timer btn-reset" v-if="!timerOn && timerPaused || timerCompleted" @click="resetTimer"></div>
    <div class="status-lamp"></div>
  </div>
</template>

<script>
export default {
  name: "TimerCard",
  data() {
    return {
      inputTime: '',
      min: 60,
      sec: 0,
      passedTime: 0,
      startedTime: null,
      timerOn: false,
      timerPaused: false,
      timerCompleted: false,
      objTimer: null,
      minForCount: null,
      secForCount: null
    }
  },
  methods: {
    startTimer() {
      if (!this.min > 0 && !this.sec > 0) {
        return false
      }
      this.startedTime = Date.now()
      this.minForCount = this.min
      this.secForCount = this.sec
      this.timerOn = true

      // setInterval内でのthisはコンポーネントインスタンスを示さない
      let self = this
      this.objTimer = setInterval(function() {
        self.count()
      }
      , 1000)
    },
    continueTimer() {
      this.timerPaused = false
      this.timerOn = true
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
    pauseTimer() {
      this.stopTimer()
      this.timerPaused = true
    },
    resetTimer() {
      this.stopTimer()
      this.timerPaused = false
      this.timerCompleted = false
    },
    count() {
      this.passedTime++

      if (this.secForCount <= 0 && this.minForCount >= 1) {
        this.minForCount--
        this.secForCount = 59
      } else if (this.secForCount <= 0 && this.minForCount <= 0) {
        this.complete()
      } else {
        this.secForCount--
      }
    },
    complete() {
      clearInterval(this.objTimer)
      this.timerCompleted = true
    }
  },
  computed: {
    formattedTime() {
      let timeString = ""
      let sMin = null
      let sSec = null
      if (!this.timerOn && !this.timerPaused) {
        sMin = String(this.min)
        sSec = String(this.sec)
      } else {
        sMin = String(this.minForCount)
        sSec = String(this.secForCount)
      }

      if (sMin.length < 2) {
        sMin = "0" + sMin
      }
      if (sSec.length < 2) {
        sSec = "0" + sSec
      }
      timeString = sMin + " : " + sSec
      return timeString
    },
    inputTimeStr: {
      get() {
        return this.inputTime
      },
      set(newTime) {
        this.inputTime = newTime
        let min = Number(this.inputTime.slice(0, -2))
        let sec = Number(this.inputTime.slice(-2))

        if (min == NaN || min < 0) {
          min = 0
        }
        if (min > 999) {
          min = 999
        }
        if (sec == NaN || sec < 0) {
          sec = 0
        }
        if (sec >= 60) {
          sec %= 60
          if (min <= 998) {
            min += 1
          }
        }
        this.min = min
        this.sec = sec
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
  box-shadow: 0 0 6px 2px #0e0e0e;
  font-size: 16px;
}

.timer-card:hover {
  .remain-time {
    top: 0;
    left: 8px;
    transform: translate(0, 0);
  }

  .btn-operate-timer {
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

.btn-operate-timer {
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

.btn-start,
.btn-continue {
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

.btn-pause {
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

.btn-reset {
  top: 40%;
  right: 8px;
  &::before,
  &::after {
    content: "";
    display: block;
    position: absolute;
  }
  &::before {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    border: 2px solid #ccc;
    border-left-color: transparent;

    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  &::after {
    width: 4.5px;
    height: 4.5px;
    clip-path: polygon(0 0, 0% 100%, 100% 100%);
    background-color: #ccc;
    top: 22%;
    left: 24%;
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

  .isActive & {
    --status-color: lightgreen;
  }
  .isPaused & {
    --status-color: orange;
  }
  .isCompleted & {
    --status-color: red;
    animation: blinking-light 2.0s infinite;
  }

  @keyframes blinking-light {
    0%, 100% {
      opacity: 1;
    }

    50% {
      opacity: 0.3;
    }

    60% {
      opacity: 1;
    }
  }
}
</style>