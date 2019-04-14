<template>
<div class="main" @mousemove="onMove()">

  <div class="welcome" v-if="sessionState === 'inactive'">
    <img src="../assets/reboot-logo.svg" alt="reboot logo"/>
    <h2>Take a break from all the noise and appreciate the beauty of silence</h2>
    <div class="controls">
      <button @click="sessionDuration=2;" :class="sessionDuration===2?'btn-active':'btn-inactive'">2 minutes</button>
      <button @click="sessionDuration=3;" :class="sessionDuration===3?'btn-active':'btn-inactive'">3 minutes</button>
      <button @click="sessionDuration=5;" :class="sessionDuration===5?'btn-active':'btn-inactive'">5 minutes</button>
    </div>
    <button @click="startSession()" class="start-btn">Start Session</button>
  </div>


  <div class="success" v-if="sessionState === 'completed'">
    <h1>You did it</h1>
    <h2>Remember, it’s okay to take a break.</h2>
    <button @click="sessionState='inactive'">Take another break?</button>
  </div>

  <transition name="fade">
    <div class="timer" v-if="sessionState === 'active'">
      <h2>{{displayTime}}</h2>
      <p>Don’t move your cursor. Just sit back, relax & breathe.</p>
    </div>
  </transition>

  <transition name="fade">
    <div class="error" v-if="error">
      <h2>Ooops! Try Again.</h2>
    </div>
  </transition>

  <footer v-if="sessionState === 'completed' || sessionState === 'inactive'">
    <a href="https://www.github.com/thelittlewonder/reboot" target="_blank">github.com/reboot</a>
  </footer>

</div>
</template>

<script>
export default {
  name: "Main",
  data() {
    return {
      displayTime: "",
      sessionDuration: 2,
      counterId: [],
      sessionState: "inactive",
      error: false
    };
  },
  methods: {
    startCounter(duration) {
      this.displayTime = "0" + this.sessionDuration + ":00";
      let timer = duration * 60,
        minutes,
        seconds;

      let counter = setInterval(() => {
        minutes = parseInt(timer / 60, 10);
        seconds = parseInt(timer % 60, 10);

        minutes = minutes < 10 ? "0" + minutes : minutes;
        seconds = seconds < 10 ? "0" + seconds : seconds;

        this.displayTime = minutes + ":" + seconds;

        if (--timer < 0) {
          timer = duration * 60;
        }

        if (this.displayTime === "00:00") {
          clearInterval(counter);
          this.sessionState = "completed";
        }
      }, 1000);

      this.counterId.push(counter);
    },
    startSession() {
      this.sessionState = "active";
      this.startCounter(this.sessionDuration);
    },
    onMove() {
      if (this.sessionState === "active") {
        this.error = true;
        //clear all the existing counters
        this.counterId.forEach(x => {
          clearInterval(x);
        });
        //call the function again
        this.startCounter(this.sessionDuration);
        window.setTimeout(() => {
          this.error = false;
        }, 2000);
      }
    }
  },
  updated() {
    //detect tab changes
    let main = this;
    window.onfocus = function() {
      console.log("tab changed");
      main.onMove();
    };
  }
};
</script>
<style lang="scss" scoped>
.main {
  border-top: 3px solid #34bf49;
  height: calc(100vh - 34px);
  background-color: #fefefe;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen-Sans, Ubuntu, Cantarell, "Helvetica Neue", sans-serif;
  .welcome {
    text-align: center;
    width: 100%;
    position: absolute;
    left: 50%;
    top: 40%;
    -webkit-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    h2 {
      font-weight: 300;
      line-height: 1.5em;
      font-size: 1.125em;
      color: #333333;
      margin: 2em 0;
      padding: 0 1em;
    }
    .controls {
      margin-bottom: 1.75em;
      display: flex;
      flex-direction: row;
      justify-content: center;
      button {
        padding: 0.5em 0.75em;
        font-weight: normal;
        line-height: normal;
        font-size: 1em;
        transition-property: all;
        transition-duration: 0.15s;
        transition-timing-function: ease-out;
        background: transparent;
        margin: 0 1em;
        cursor: pointer;
        border-radius: 3px;
        &:focus {
          outline: none;
        }
      }
    }
    .btn-active {
      border: 1px solid #34bf49;
      color: #34bf49;
    }
    .btn-inactive {
      border: 1px solid rgba(0, 0, 0, 0.15);
      color: rgba(0, 0, 0, 0.25);
    }
    .start-btn {
      font-weight: 500;
      line-height: normal;
      font-size: 18px;
      letter-spacing: 0.5px;
      color: #ffffff;
      padding: 0.75em 1em;
      background: #34bf49;
      box-shadow: 0px 1px 0px rgba(41, 169, 61, 0.15);
      border-radius: 3px;
      cursor: pointer;
      border: transparent;
      &:focus {
        outline: 1px solid rgba(#34bf49, 0.5);
      }
    }
  }
  footer {
    text-align: center;
    height: 34px;
    position: absolute;
    bottom: 0;
    left: 50%;
    -webkit-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    a {
      text-decoration: none;
      color: rgba(#000, 0.25);
      font-weight: 300;
      transition-property: all;
      transition-duration: 0.25s;
      transition-timing-function: ease-in-out;
      &:hover {
        color: #34bf49;
      }
    }
  }
  .timer {
    position: absolute;
    left: 50%;
    top: 40%;
    width: 100%;
    -webkit-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    text-align: center;
    h2 {
      font-weight: 300;
      line-height: normal;
      font-size: 3.5em;
      color: #34bf49;
    }
    p {
      font-weight: 300;
      line-height: 1.5em;
      font-size: 1.125em;
      color: #333333;
      margin-top: 1em;
      padding: 0 0.5em;
    }
  }
  .error {
    position: absolute;
    left: 50%;
    top: 52%;
    -webkit-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    h2 {
      text-align: center;
      font-weight: normal;
      line-height: normal;
      font-size: 1.125em;
      color: #ff4c4c;
    }
  }
  @media screen and (max-width: 768px) {
    .error {
      top: 55% !important;
    }
  }
  .success {
    position: absolute;
    left: 50%;
    top: 40%;
    width: 100%;
    -webkit-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    text-align: center;
    h1 {
      font-weight: 300;
      line-height: normal;
      font-size: 2em;
      color: #34bf49;
    }
    h2 {
      margin: 1em 2em;
      font-weight: 300;
      line-height: 1.5em;
      font-size: 1.125em;
      color: #333333;
    }
    button {
      font-weight: normal;
      line-height: normal;
      font-size: 1em;
      color: #34bf49;
      padding: 0.5em 0.75em;
      background: transparent;
      border: 1px solid #34bf49;
      border-radius: 2px;
      cursor: pointer;
      &:focus {
        outline: none;
      }
    }
  }
}
</style>
