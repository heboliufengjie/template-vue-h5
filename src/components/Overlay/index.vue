<template>
  <transition name="li-fade">
    <div v-show="show" :class="cls" @click="_click" @touchmove="_touch">
      <slot></slot>
    </div>
  </transition>
</template>

<script>
import { throttle } from "@u/decorator";
export default {
  name: "LiOverlay",
  props: {
    show: {
      type: Boolean
    },
    // 锁定滚动
    lockScroll: {
      type: Boolean,
      default: true
    },
    // 类型，black 和 white
    type: {
      type: String,
      default: "black"
    }
  },
  computed: {
    cls() {
      return {
        "li-overlay": true,
        "li-overlay__white": this.type === "white"
      };
    }
  },
  methods: {
    @throttle(300, true)
    _click() {
      this.$emit("click", ...arguments);
    },
    _touch(e) {
      if (this.lockScroll) {
        e.preventDefault();
      }
    }
  }
};
</script>

<style lang="scss">
.li-overlay {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1000;
  width: 100%;
  height: 100%;
  background-color: rgba($color: black, $alpha: 0.7);
}

.li-overlay__white {
  background-color: rgba($color: white, $alpha: 0.7);
}
</style>