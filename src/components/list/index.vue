<template>
  <div class="list-container">
    <slot></slot>
    <div v-show="loading" class="load">加载中...</div>

  </div>
</template>

<script type="text/ecmascript-6">
import utils from "../utils/scroll";
import { on, off } from "../utils/event";
export default {
  name: "",
  data() {
    return {};
  },
  model: {
    prop: "loading"
  },

  props: {
    loading: Boolean,
    finished: Boolean,
    immediateCheck: {
      type: Boolean,
      default: true
    },
    offset: {
      type: Number,
      default: 300
    },
    loadingText: String
  },
  mounted() {
    this.scroller = utils.getScrollEventTarget(this.$el);
    this.handler(true);
    console.log('this.loading',this.loading)

    if (this.immediateCheck) {
      this.$nextTick(this.check);
    }
  },

  destroyed() {
    this.handler(false);
  },

  activated() {
    this.handler(true);
  },

  deactivated() {
    this.handler(false);
  },

  watch: {
    loading() {
      this.$nextTick(this.check);
    },

    finished() {
      this.$nextTick(this.check);
    }
  },
  methods: {
    check() {
      if (this.loading || this.finished) {
        return;
      }

      const el = this.$el;
      const { scroller } = this;
      const scrollerHeight = utils.getVisibleHeight(scroller);

      /* istanbul ignore next */
      if (
        !scrollerHeight ||
        utils.getComputedStyle(el).display === "none" ||
        el.offsetParent === null
      ) {
        return;
      }

      const scrollTop = utils.getScrollTop(scroller);
      const targetBottom = scrollTop + scrollerHeight;

      let reachBottom = false;

      /* istanbul ignore next */
      // if (el === scroller) {
        // console.log('el === scroller')
        if (scroller === window) reachBottom = document.documentElement.scrollHeight - targetBottom < this.offset;
        else 
        reachBottom = scroller.scrollHeight - targetBottom < this.offset;
       /*  console.log('scrollHeight', scroller.scrollHeight)
        console.log('scrollTop', scrollTop)
        console.log('targetBottom', targetBottom)
        console.log('scroller', scroller)
        console.log('reachBottom', reachBottom) */
     /*  } else {
        const elBottom =
          utils.getElementTop(el) -
          utils.getElementTop(scroller) +
          utils.getVisibleHeight(el);
        reachBottom = elBottom - scrollerHeight < this.offset;
      } */

      /* istanbul ignore else */
      if (reachBottom) {
        this.$emit("input", true);
        this.$emit("load");
      }
    },

    handler(bind) {
      /* istanbul ignore else */
      if (this.binded !== bind) {
        this.binded = bind;
        (bind ? on : off)(this.scroller, "scroll", this.check);
      }
    }
  }
};
</script>

<style scoped lang="stylus">
</style>
