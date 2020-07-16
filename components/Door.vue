<template>
  <div class="door-container">
    <transition name="door-transition">
      <div v-if="!open" class="door">
        <div class="frame"></div>
        <div class="handle" @click="openDoor"></div>
      </div>
    </transition>
    <div v-if="open">
      <Modal @close="closeDoor">
        <h3 slot="header">This is Telehealth</h3>
        <div slot="body">
          <Telehealth :name="name" />
        </div>
        <div slot="footer"></div>
      </Modal>
    </div>
    <div @click="closeDoor" class="door door--open"></div>
  </div>
</template>
<script>
import Modal from "./Modal";
import Telehealth from "./Telehealth";
export default {
  name: "Door",
  components: {
    Modal,
    Telehealth
  },
  props: {
    name: {
      required: true,
      default: null
    }
  },
  data() {
    return {
      open: false
    };
  },

  methods: {
    openDoor() {
      this.open = true;
    },

    closeDoor() {
      this.open = false;
    }
  }
};
</script>
<style scoped>
.door-container {
  position: absolute;
  bottom: 0;
  left: 0;
  height: 100%;
  width: 100%;
  display: flex;
  align-items: flex-end;
  justify-content: center;
}
.door {
  max-width: 300px;
  width: 50%;
  margin: 0 auto;
  background: #64534b;
  height: 70%;
  max-height: 400px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  z-index: 1;
  transform-origin: left;
}
.frame {
  width: 97%;
  height: 97%;
  border: double 3px lightgray;
}
.handle {
  cursor: pointer;
  position: absolute;
  height: 30px;
  width: 30px;
  border-radius: 50%;
  top: calc(50% - 5px);
  right: 20px;
  background: gold;
  display: flex;
  align-items: center;
  justify-content: center;
}
.handle:after {
  content: "";
  background: black;
  border-radius: 50%;
  width: 7px;
  height: 7px;
}
.handle:before {
  position: absolute;
  left: calc(50% - 3px);
  z-index: 1;
  content: "";
  background: gold;
  border-radius: 50%;
  width: 6px;
  height: 2px;
}
.door--open {
  background: #000;
  z-index: 0;
}
/* Enter and leave animations can use different */
/* durations and timing functions.              */
.door-transition-enter-active {
  transition: all 0.3s linear;
}
.door-transition-enter {
  /* .door-transition-leave-active below version 2.1.8 */
  transform-origin: left;
  transform: perspective(1200px) rotateY(90deg);
}
.door-transition-leave-active {
  transition: all 0.3s linear;
  transform: perspective(1200px) rotateY(90deg);
}
</style>
