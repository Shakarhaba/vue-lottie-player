<template>
  <div
    class="vue-lottie-container"
    :style="style"
    ref="container"
    v-on:click="click"
    v-on:dblclick="dblclick"
  ></div>
</template>

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
  watch: {
    animationData() {
      this.loadAnimation();
    },
  },
  methods: {
    replaceOnLoopComplete(replacement, options = {}) {
      this.$on("loopComplete", () => {
        this.pause();
        this.destroy();
        this.loadAnimation(replacement, options);
      });
    },
    click(e) {
      this.$emit("click", e);
    },
    dblclick(e) {
      this.$emit("dblclick", e);
    },
    mute() {
      if (this.animation !== null) {
        this.animation.mute();
      }
    },
    unmute() {
      if (this.animation !== null) {
        this.animation.unmute();
      }
    },
    volume(volume) {
      if (this.animation !== null) {
        this.animation.setVolume(volume);
      }
    },
    play() {
      if (this.animation !== null) {
        this.animation.play();
      }
    },
    reset() {
      if (this.animation !== null) {
        this.animation.goToAndStop(0);
      }
    },
    replay() {
      if (this.animation !== null) {
        this.animation.goToAndStop(0);
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
    togglePause() {
      if (this.animation !== null) {
        this.animation.togglePause();
      }
    },
    speed(speed) {
      if (this.animation !== null) {
        this.animation.setSpeed(Number(speed));
      }
    },
    setDirection(direction) {
      if (this.animation !== null) {
        this.animation.setDirection(direction);
      }
    },
    goToAndStop(frame) {
      if (this.animation !== null) {
        this.animation.goToAndStop(frame);
      }
    },
    destroy(frame) {
      if (this.animation !== null) {
        this.animation.destroy();
      }
    },
    getSize(size) {
      return typeof size === "number" ? `${size}px` : size;
    },
    loadAnimation(animationData = null, options = {}) {
      const lottieConfig = Object.assign(
        {
          container: this.$refs.container,
          name: this.name,
          renderer: this.renderer,
          loop: this.loop,
          autoplay: this.autoplay,
          width: this.getSize(this.width),
          height: this.getSize(this.height),
          path: this.path,
        },
        options
      );
      lottieConfig.animationData = animationData || this.animationData;

      this.animation = lottie.loadAnimation(lottieConfig);

      this.animation.addEventListener("complete", (e) => {
        this.$emit("complete", { node: this, event: e });
      });
      this.animation.addEventListener("loopComplete", (e) => {
        this.$emit("loopComplete", { node: this, event: e });
      });
      this.animation.addEventListener("enterFrame", (e) => {
        this.$emit("enterFrame", { node: this, event: e });
      });
      this.animation.addEventListener("segmentStart", (e) => {
        this.$emit("segmentStart", { node: this, event: e });
      });
      this.animation.addEventListener("config_ready", (e) => {
        this.$emit("configReady", { node: this, event: e });
      });
      this.animation.addEventListener("data_ready", (e) => {
        this.$emit("dataReady", { node: this, event: e });
      });
      this.animation.addEventListener("DOMLoaded", (e) => {
        this.$emit("DOMLoaded", { node: this, event: e });
      });
      this.animation.addEventListener("destroy", (e) => {
        this.$emit("destroy", { node: this, event: e });
      });

      this.$emit("lottie", this.animation);
    },
  },
};
</script>
