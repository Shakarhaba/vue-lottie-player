<script>
import lottie from "lottie-web";

export default {
  props: {
    name: {
      type: String,
      default: () => "lottie-" + Math.random(),
    },
    width: {
      type: [String, Number],
      default: () => "200px",
    },
    height: {
      type: [String, Number],
      default: () => "200px",
    },
    background: {
      type: String,
      default: "transparent",
    },

    loop: {
      type: [Boolean, Number],
      default: () => false,
    },
    autoplay: {
      type: Boolean,
      default: () => true,
    },
    renderer: {
      type: String,
      default: () => "svg",
    },
    path: {
      type: String,
      default: () => null,
    },
    animationData: {
      type: Object,
      default: () => null,
    },
  },

  data: (vm) => ({
    style: {
      width: vm.getSize(vm.width),
      height: vm.getSize(vm.height),
      background: vm.background,
    },
    animation: null,
  }),

  mounted() {
    this.loadAnimation();
  },

  methods: {
    play() {
      if (this.animation !== null) {
        this.animation.play();
      }
    },
    stop() {
      if (this.animation !== null) {
        this.animation.stop();
      }
    },
    pause() {
      if (this.animation !== null) {
        this.animation.pause();
      }
    },
    getSize(size) {
      return typeof size === "number" ? `${size}px` : size;
    },
    loadAnimation() {
      let animation = lottie.loadAnimation({
        container: this.$refs.container,
        name: this.name,
        renderer: this.renderer,
        loop: this.loop,
        autoplay: this.autoplay,
        width: this.getSize(this.width),
        height: this.getSize(this.height),
        path: this.path,
        animationData: this.animationData,
      });

      this.animation = animation;
      this.$emit("lottie", animation);
    },
  },
};
</script>

<template>
  <div :style="style" ref="container"></div>
</template>
