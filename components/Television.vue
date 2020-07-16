<template>
  <div>
    <div v-show="this.tvOn">
      <Modal @close="tvOn = false">
        <h3 slot="header">ToBeSlider</h3>
        <div slot="body">
          <iframe width="560" height="315" src="https://www.youtube.com/embed/oHg5SJYRHA0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        </div>
        <div slot="footer"></div>
      </Modal>
    </div>
    <div class="tv-container" @click="turnOnTv">
      <img src="/trees.jpg" />
    </div>
  </div>
</template>
<script>
import Modal from "./Modal";
export default {
  name: "Television",
  components: {
    Modal
  },
  data() {
    return {
      tvOn: false
    };
  },
  methods: {
    turnOnTv() {
      this.tvOn === false ? (this.tvOn = true) : (this.tvOn = false);
    }
  }
};
</script>
<style scoped>
.tv-container {
  position: absolute;
  background: #000;
  height: 100px;
  width: 150px;
  max-width: 100%;
  left: calc(50% - 75px);
  top: calc(50% - 50px);
  cursor: pointer;
}
img {
  height: 100%;
  width: 100%;
  padding: 5px;
}
</style>
