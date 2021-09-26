<template>
  <div class="timer-card">
    <div class="btn-stop" v-if="timerOn" @click="stopTimer">STOP</div>
    <div class="remain-time">{{formatTime}}</div>
    <input type="number" class="input-set-time no-spin" placeholder="3000 -> 30:00" max="9959">
    <div class="btn-start"></div>
    <div class="status-lamp" :class="(timerOn) ? 'is-active' : ''"></div>
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
  
  transition: all 0.1s linear;
}

.btn-stop {
  display: flex;
  position: absolute;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  border-radius: 16px;
  align-items: center;
  justify-content: center;

  visibility: hidden;
  opacity: 0;
  transition: all 0.2s ease-in-out;
}

.btn-stop {
  background-color: rgba(33, 33, 33, 0.8);
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