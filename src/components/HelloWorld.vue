<template>
  <div>
    <div class="imageHolder">
      <img class="showInCenter" src="./1.jpg" alt="">
      <img class="hideRight" src="./2.jpg" alt="">
      <img class="hideRight" src="./3.jpg" alt="">
      <img class="hideRight" src="./4.jpg" alt="">
      <img class="hideRight" src="./5.jpg" alt="">
    </div>

    <div>
      <button :disabled="isDisabled" @click="showPreviousImage" class="btn">Left</button>
      <button :disabled="isDisabled" @click="showNextImage" class="btn">Right</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      allChildren: [],
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
      return (this.index !== this.allChildren.length)
        ? this.nextNormal()
        : this.nextExeption();
    },
    nextNormal() {
      this.allChildren[this.index - 1].classList = "moveLeft";
      return this.allChildren[this.index].classList = "showInCenter";
    },
    nextExeption() {
      this.index = 0;
      this.allChildren[this.allChildren.length - 1].classList = "moveLeft";
      this.allChildren[0].classList = "hideRight";
      return setTimeout(() => {
        this.allChildren[0].classList = "showInCenter";
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
      this.allChildren[this.index + 1].classList = "moveRight";
      return this.allChildren[this.index].classList = "showInCenter";
    },
    prevExeption() {
      this.allChildren[0].classList = "moveRight";
      return setTimeout(() => {
        this.allChildren[this.allChildren.length - 1].classList = "showInCenter";
        this.index = this.allChildren.length - 1;
      }, 0);
    },
    disableFastClickOnBtn() {
      this.isDisabled = true;
      return setTimeout(() => {
        this.isDisabled = false;
      }, this.animationTime);
    },
    placeOtherImagesInWaitingPosition(className) {
      return this.allChildren.forEach((div, idx) => (idx !== this.index) ? div.classList = className : null);
    }
  },
  mounted() {
    return this.allChildren = Array.from(document.getElementsByClassName("imageHolder")[0].children);
  }
}
</script>

<style scoped lang="sass">
.imageHolder
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
  &:disabled
    color: black
</style>
