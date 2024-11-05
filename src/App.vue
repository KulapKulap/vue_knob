<template>
  <div
    class="knob"
    :style="{
      width: size + 'px',
      height: size + 'px',
    }"
    @mousedown="startDrag"
    @touchstart="startDrag"
  >
    <!-- Circular Progress -->
    <svg
      class="knob-circle"
      :width="size"
      :height="size"
      viewBox="0 0 100 100"
    >
      <circle
        class="knob-track"
        cx="50"
        cy="50"
        r="45"
        :stroke="trackColor"
        stroke-width="10"
        fill="none"
      />
      <circle
        class="knob-progress"
        cx="50"
        cy="50"
        r="45"
        :stroke="progressColor"
        stroke-width="10"
        fill="none"
        :style="{ strokeDasharray: circumference, strokeDashoffset: progressOffset }"
      />
    </svg>
    <div class="knob-value" :style="{ color: valueColor }">{{ currentValue }}</div>
  </div>
</template>

<script>
export default {
  name: "Knob",
  props: {
    min: { type: Number, default: 0 },
    max: { type: Number, default: 100 },
    size: { type: Number, default: 100 },
    trackColor: { type: String, default: "#ccc" },
    progressColor: { type: String, default: "#00f" },
    valueColor: { type: String, default: "#000" },
    initial: { type: Number, default: 0 },
  },
  data() {
    return {
      currentValue: this.initial,
      isDragging: false,
      circumference: 2 * Math.PI * 45, // Radius is 45
    };
  },
  computed: {
    progressOffset() {
      const percentage = (this.currentValue - this.min) / (this.max - this.min);
      return this.circumference * (1 - percentage);
    },
  },
  methods: {
    startDrag(event) {
      this.isDragging = true;
      document.addEventListener("mousemove", this.handleDrag);
      document.addEventListener("mouseup", this.stopDrag);
      document.addEventListener("touchmove", this.handleDrag);
      document.addEventListener("touchend", this.stopDrag);
    },
    handleDrag(event) {
      if (!this.isDragging) return;
      const { clientX, clientY } = event.touches ? event.touches[0] : event;
      const rect = this.$el.getBoundingClientRect();
      const centerX = rect.left + rect.width / 2;
      const centerY = rect.top + rect.height / 2;
      const angle = Math.atan2(clientY - centerY, clientX - centerX) * (180 / Math.PI) + 90;
      const normalizedAngle = (angle + 360) % 360;

      const value = Math.round(
        this.min + ((normalizedAngle / 360) * (this.max - this.min))
      );

      this.currentValue = Math.max(this.min, Math.min(this.max, value));
      this.$emit("input", this.currentValue);
    },
    stopDrag() {
      this.isDragging = false;
      document.removeEventListener("mousemove", this.handleDrag);
      document.removeEventListener("mouseup", this.stopDrag);
      document.removeEventListener("touchmove", this.handleDrag);
      document.removeEventListener("touchend", this.stopDrag);
    },
  },
};
</script>

<style scoped>
.knob {
  position: relative;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  user-select: none;
}
.knob-circle {
  position: absolute;
  top: 0;
  left: 0;
  transform: rotate(-90deg);
}
.knob-track {
  opacity: 0.3;
}
.knob-progress {
  transition: stroke-dashoffset 0.3s ease;
}
.knob-value {
  font-size: 1.5em;
  pointer-events: none;
  z-index: 1;
}
</style>
