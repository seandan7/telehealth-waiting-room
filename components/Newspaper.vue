<template>
  <div>
    <div v-show="this.newsOn">
      <Modal @close="newsOn = false">
        <h3 slot="header">COMPANY In the News</h3>
        <div slot="body">
          <div v-if="itemActive">
            <div>
              <h1>{{activeItem.title}}</h1>
              <div>{{activeItem.message}}</div>
              <button @click="returnToItems">Back</button>
            </div>
          </div>
          <div v-else>
            <div
              @click="makeItemActive(item)"
              class="news--item"
              v-for="item in items"
              :key="item.id"
            >
              {{ item.preview }}
            </div>
          </div>
        </div>
        <div slot="footer"></div>
      </Modal>
    </div>
    <div class="newspaper-container" @click="turnOnNews">
      <div class="newspaper">
        <img src="/paper.jpg" />
      </div>
    </div>
  </div>
</template>
<script>
import Modal from "./Modal";
export default {
  name: "Newspaper",
  components: {
    Modal
  },
  data() {
    return {
      newsOn: false,
      itemActive: false,
      activeItem: null,
      items: [
        {
          id: 1,
          title: "We won an award",
          message: "We won an award for blah",
          preview: "We won an award..."
        },
        {
          id: 2,
          title: "Updates to our Covid Policy",
          message: "We have updated our covid policy to remain blah blah",
          preview: "We have updated our policy..."
        },
        {
          id: 3,
          title: "Check out our new Telehealth",
          message: "We have made a new telehealth blah blah",
          preview: "We have made a new tele..."
        }
      ]
    };
  },
  methods: {
    makeItemActive(item) {
      this.itemActive = true;
      this.activeItem = item;
    },
    turnOnNews() {
      this.newsOn === false ? (this.newsOn = true) : (this.newsOn = false);
    },
    returnToItems() {
      this.activeItem = null;
      this.itemActive = false;
    }
  }
};
</script>
<style scoped>
.newspaper-container {
  position: absolute;
  background: url("/floor.jpg");
  width: 100px;
  height: 150px;
  top: calc(50% - 75px);
  left: calc(50% - 50px);
  cursor: pointer;
}
img {
  width: 100%;
  height: 100%;
  padding: 5px;
}
.news--item {
  border-bottom: 2px solid lightgray;
  padding: 5px 0;
  font-weight: bold;
  color: darkblue;
  text-align: left;
}
.news--item:last-child {
  border: none;
}
</style>
