<template>
  <div class="timeline"
       :style="{background: calculateGradient()}"
  >
    <span v-for="count in counts"
          :key="`text${count}`"
          :style="{left: calculatePositionText(counts, count)}"
          class="timeline__step-text"
          :class="{'timeline__step-text--under': isTextUnder}"
    >
      Step {{ count }}
    </span>
    <div v-for="count in counts"
         :key="count"
         :style="{left: calculatePositionStep(counts, count)}"
         class="timeline__step"
         :class="{
           'timeline__step--process': isStepProcess(count),
           'timeline__step--incomplete': isStepIncomplete(count),
          }"
    ></div>
  </div>
</template>

<script>
export default {
  name: 'appTimeline',
  props: {
    counts: {
      type: Number,
      default: 0,
    },
    processCount: {
      type: Number,
      default: 0,
    },
    isTextUnder: {
      type: Boolean,
      default: false,
    },
    isTextCenter: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    calculatePositionStep(counts, count) {
      if (this.isTextCenter) {
        return `calc(100% / ${counts} * ${count})`;
      } else {
        return `calc(100% / ${counts - 1} * ${count})`;
      }
    },
    calculatePositionText(counts, count) {
      if (this.isTextCenter) {
        return `calc((100% / ${counts} * ${count}) - (100% / ${counts} / 2)`;
      } else {
        return `calc((100% / ${counts} * ${count - 1})`;
      }
    },
    isStepProcess(count) {
      return this.processCount === (count + 1);
    },
    isStepIncomplete(count) {
      return this.processCount < (count + 1);
    },
    calculateGradient() {
      if (this.isTextCenter) {
        return `linear-gradient(to right, #4EA31E calc(100% / ${this.counts} * ${this.processCount - 1}), #F49530 calc(100% / ${this.counts} * ${this.processCount - 1}), #F49530 calc(100% / ${this.counts} * ${this.processCount}), #E5E5E5 calc(100% / ${this.counts} * ${this.processCount}))`;
      } else {
        return `linear-gradient(to right, #4EA31E calc(100% / ${this.counts} * ${this.processCount}), #F49530 calc(100% / ${this.counts} * ${this.processCount})`;
      }
    },
  },
};
</script>

<style scoped lang="scss">
.timeline {
  position: relative;
  width: 100%;
  height: 3px;

  &::before {
    left: 0;
    content: '';
  }

  &__step-text {
    position: absolute;
    color: #B094A2;
    font-family: sans-serif;
  }

  &__step-text:not(&__step-text--under) {
    top: 0;
    padding-bottom: 10px;
    transform: translate(-50%, -100%);
  }

  &__step-text--under {
    bottom: 0;
    padding-top: 10px;
    transform: translate(-50%, 100%);
  }

  &__step,
  &::before {
    position: absolute;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 15px;
    height: 15px;
    border-radius: 50%;
    border: 2px solid #FFFFFF;
    background-color: #4EA31E;
  }

  &__step--process {
    background-color: #F49530;
  }

  &__step--incomplete {
    background-color: #FFFFFF;
    border-color: #E5E5E5;
  }
}
</style>