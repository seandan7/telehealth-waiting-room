<template>
  <div class="room-container">
    <transition name="fade-top">
      <div v-if="!papersDone">
        <div class="whitepaper">
          <h3>Sign In</h3>
          Name: <input type="text" v-model="patientName" id="name" />
          <a href="#" @click="signInDone">Done</a>
        </div>
      </div>
    </transition>
    <div class="wall wall--left">
      <h2>Watch TV</h2>
      <Television />
    </div>
    <div class="wall wall--center">
      <h1>COMPANY Virtual Waiting Room</h1>
      <p>
        Your doctor is
        {{ this.doctorReady ? "ready for you" : "not yet available" }}
      </p>
      <Door :name="patientName" />
    </div>
    <div class="wall wall--right">
      <h2>COMPANY News</h2>
      <Newspaper />
    </div>
    <Floor @nameRecieved="giveRoomName" />
  </div>
</template>
<script>
import Door from "./Door";
import Floor from "./Floor";
import Newspaper from "./Newspaper";
import Television from "./Television";
export default {
  name: "Room",
  components: {
    Door,
    Floor,
    Newspaper,
    Television
  },
  data() {
    return {
      doctorReady: false,
      patientName: null,
      papersDone: false
    };
  },

  methods: {
    signInDone() {
      this.papersDone = true;
    },
    giveRoomName(name) {
      this.patientName = name;
    }
  }
};
</script>
<style scoped lang="scss">
.room-container {
  width: 100%;
  height: 80vh;
  display: flex;
  flex-wrap: wrap;
}
.wall {
  overflow: hidden;
  position: relative;
  height: 100%;
  background: lightblue;
  &--left {
    width: 20%;
    border-right: 2px solid darkgray;
  }
  &--center {
    width: 60%;
  }
  &--right {
    width: 20%;
    border-left: 2px solid darkgray;
  }
}
.wall h1,
.wall h2 {
  padding-top: 20px;
}
.whitepaper {
  max-width: 100%;
  position: absolute;
  height: 500px;
  bottom: 0;
  background: white;
  width: 400px;
  padding: 5px;
  text-align: left;
  box-shadow: 2px 2px 2px #000;
  margin: 0 auto;
  z-index: 10000;
  left: calc(50% - 200px);
}

/* Enter and leave animations can use different */
/* durations and timing functions.              */
.fade-top-enter-active {
  transition: all 0.3s ease;
}
.fade-top-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.fade-top-enter, .fade-top-leave-to
/* .fade-top-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
