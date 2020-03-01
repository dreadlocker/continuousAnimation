<template>
  <div>
    <div class="imagesHolder">
      <img 
        v-for="(img, index) in imgsArr"
        :key="index"
        :src="img"
        ref="singleImage"
        :class="index === 0 ? 'showInCenter' : 'hideRight'"
      >
    </div>

    <button :disabled="isDisabled" @click="showPreviousImage" class="btn">Left</button>
    <button :disabled="isDisabled" @click="showNextImage" class="btn">Right</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      imgsArr: [
        require("./1.jpg"),
        require("./2.jpg"),
        require("./3.jpg"),
        require("./4.jpg"),
        require("./5.jpg"),
      ],
      allImagesArr: [],
      index: 0,
      animationTime: 500,
      isDisabled: false
    }
  },
  methods: {
    showNextImage() {
      this.disableFastClickOnBtn();
      this.placeOtherImagesInWaitingPosition("hideRight");
      this.index++;
      return (this.index !== this.allImagesArr.length)
        ? this.nextNormal()
        : this.nextExeption();
    },
    nextNormal() {
      this.allImagesArr[this.index - 1].classList = "moveLeft";
      return this.allImagesArr[this.index].classList = "showInCenter";
    },
    nextExeption() {
      this.index = 0;
      this.allImagesArr[this.allImagesArr.length - 1].classList = "moveLeft";
      this.allImagesArr[0].classList = "hideRight";
      return setTimeout(() => {
        this.allImagesArr[0].classList = "showInCenter";
        this.index = 0;
      }, 0);
    },
    showPreviousImage() {
      this.disableFastClickOnBtn();
      this.placeOtherImagesInWaitingPosition("hideLeft");
      return (this.index > 0)
        ? this.prevNormal()
        : this.prevExeption();
    },
    prevNormal() {
      this.index--;
      this.allImagesArr[this.index + 1].classList = "moveRight";
      return this.allImagesArr[this.index].classList = "showInCenter";
    },
    prevExeption() {
      this.allImagesArr[0].classList = "moveRight";
      return setTimeout(() => {
        this.allImagesArr[this.allImagesArr.length - 1].classList = "showInCenter";
        this.index = this.allImagesArr.length - 1;
      }, 0);
    },
    disableFastClickOnBtn() {
      this.isDisabled = true;
      return setTimeout(() => {
        this.isDisabled = false;
      }, this.animationTime);
    },
    placeOtherImagesInWaitingPosition(className) {
      return this.allImagesArr.forEach((div, idx) => (idx !== this.index) ? div.classList = className : null);
    }
  },
  mounted() {
    return this.allImagesArr = this.$refs.singleImage;
  }
}
</script>

<style scoped lang="sass">
.imagesHolder
  width: 256px // image width
  height: 256px // image width
  position: relative
  overflow: hidden
  margin: auto
.showInCenter
  position: absolute
  top: 0
  left: 0
  transition: all 0.5s ease
.hideRight
  position: absolute
  left: 256px // image width
.moveLeft
  position: absolute
  left: calc( (-256px) - 2rem  ) // image width
  transition: all 0.5s ease

.moveRight
  position: absolute
  left: calc( (256px) + 2rem  ) // image width
  transition: all 0.5s ease
.hideLeft
  position: absolute
  left: -256px // image width

.btn
  margin-top: 1rem
  &:disabled
    color: black
</style>
