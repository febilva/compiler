<template>
  <div class="split-pane"
    @mousemove="dragMove"
    @mouseup="dragEnd"
    @mouseleave="dragEnd"
    :class="{ dragging: dragging }">
    <div class="left" :style="{ width: split + '%' }">
      <slot name="left"></slot>
      <div class="dragger" @mousedown="dragStart">
      </div>
    </div>
    <div class="right" :style="{ width: (100 - split) + '%' }">
      <slot name="right"></slot>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      split: 50,
      dragging: false
    }
  },
  methods: {
    dragStart (e) {
      this.dragging = true
      this.startX = e.pageX
      this.startSplit = this.split
    },
    dragMove (e) {
      if (this.dragging) {
        const dx = e.pageX - this.startX
        const totalWidth = this.$el.offsetWidth
        this.split = this.startSplit + ~~(dx / totalWidth * 100)
      }
    },
    dragEnd () {
      this.dragging = false
    }
  }
}
</script>

<style scoped>
.split-pane {
	display: flex;
	height: 100%;
  width: 100%;
}

.split-pane.dragging {
	cursor: ew-resize;
}

.left, .right {
	position: relative;
  width: 100%;
}

.left {
	border-right: 1px solid #333;
}

.dragger {
	position: absolute;
	z-index: 99;
	top: 0;
	bottom: 0;
	right: -5px;
	width: 10px;
	cursor: ew-resize;
}
</style>
