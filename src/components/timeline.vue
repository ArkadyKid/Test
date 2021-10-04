<template>
  <div class="timeline"
       :style="{ background: calculateGradient }"
       :class="{ 'timeline--text-on-checkpoint': !isTextCenterProgress }"
  >
    <template v-for="count in counts">
      <span :key="`text${count}`"
            :style="{ left: calculatePositionText(counts, count) }"
            class="timeline__step-text"
            :class="{
              'timeline__step-text--under': isTextUnder,
              'timeline__step-text--checkpoint': !isTextCenterProgress,
            }"
      >
        Step {{ count }}
      </span>
      <div v-if="isTextCenterProgress && count === 1"
           class="timeline__step timeline__step--first"
           :class="{' timeline__step--first-active': isStepFirstActive(count) }"
           :key="`first${count}`"
      ></div>
      <div :key="count"
           :style="{ left: calculatePositionStep(counts, count) }"
           class="timeline__step"
           :class="{
             'timeline__step--checkpoint': isStepCheckpoint(count),
             'timeline__step--incomplete': isStepIncomplete(count),
            }"
      ></div>
    </template>

    <div class="timeline__options">
      <label class="timeline__option-label">
        <input class="timeline__option-checkbox"
               type="checkbox"
               name="textIsUnder"
               v-model="isTextUnder"
        >
        Text {{ positionLabelText }} timeline
      </label>
      <label class="timeline__option-label">
        <input class="timeline__option-checkbox"
               type="checkbox"
               name="textIsCenterProgress"
               v-model="isTextCenterProgress"
        >
        Text on center {{ positionLabelCenterText }}
      </label>
      <label class="timeline__option-label">
        <input class="timeline__option-input"
               type="number"
               name="stepCounts"
               v-model.number="counts"
               min="1"
               max="20"
        >
        Step counts
      </label>
      <label class="timeline__option-label">
        <input class="timeline__option-input"
               type="number"
               name="activeCount"
               v-model.number="activeCheckpoint"
               min="1"
               :max="counts"
        >
        Active count
      </label>
    </div>
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
    activeCheckpoint: {
      type: Number,
      default: 0,
    },
    isTextUnder: {
      type: Boolean,
      default: false,
    },
    isTextCenterProgress: {
      type: Boolean,
      default: false,
    },
  },

  methods: {
    calculatePositionStep(counts, count) {
      if (this.isTextCenterProgress) {
        return `calc(100% / ${counts} * ${count})`;
      } else {
        return `calc(100% / ${counts - 1} * ${count - 1})`;
      }
    },
    calculatePositionText(counts, count) {
      if (this.isTextCenterProgress) {
        return `calc((100% / ${counts} * ${count}) - (100% / ${counts} / 2)`;
      } else {
        return `calc((100% / ${counts - 1} * ${count - 1})`;
      }
    },
    isStepCheckpoint(count) {
      if (this.isTextCenterProgress) {
        return this.activeCheckpoint === (count + 1);
      } else {
        return this.activeCheckpoint === count;
      }
    },
    isStepIncomplete(count) {
      if (this.isTextCenterProgress) {
        return this.activeCheckpoint < (count + 1);
      } else {
        return this.activeCheckpoint < count;
      }
    },
    isStepFirstActive(count) {
      if (this.isTextCenterProgress) {
        return this.activeCheckpoint === count;
      }
    },
  },

  computed: {
    positionLabelCenterText() {
      return this.isTextCenterProgress ? 'progress' : 'checkpoint';
    },
    positionLabelText() {
      return this.isTextUnder ? 'under' : 'above';
    },
    calculateGradient() {
      if (this.isTextCenterProgress) {
        return `linear-gradient(to right, #4EA31E calc(100% / ${this.counts} * ${this.activeCheckpoint - 1}), #F49530 calc(100% / ${this.counts} * ${this.activeCheckpoint - 1}), #F49530 calc(100% / ${this.counts} * ${this.activeCheckpoint}), #E5E5E5 calc(100% / ${this.counts} * ${this.activeCheckpoint}))`;
      } else {
        return `linear-gradient(to right, #4EA31E calc(100% / ${this.counts - 1} * ${this.activeCheckpoint - 1}), #E5E5E5 calc(100% / ${this.counts - 1} * ${this.activeCheckpoint - 1})`;
      }
    },
  },
};
</script>

<style scoped lang="scss">
.timeline {
  position: relative;
  width: 100%;
  height: 2px;
  color: #B094A2;
  font-family: 'Arial', sans-serif;
  font-size: 12px;

  &__step-text {
    position: absolute;
    white-space: nowrap;
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

  &__step {
    position: absolute;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 10px;
    height: 10px;
    border-radius: 50%;
    border: 2px solid transparent;
    box-shadow: 0 0 0 2px #FFFFFF;
    background-color: #4EA31E;
  }

  &__step--first {
    left: 0;
  }

  &__step--checkpoint,
  &__step--first-active {
    background-color: #F49530;
  }

  &__step--incomplete {
    background-color: #FFFFFF;
    border-color: #E5E5E5;
  }

  &--text-on-checkpoint &__step-text:first-of-type {
    transform: translate(0, -100%);
  }

  &--text-on-checkpoint &__step-text:last-of-type {
    transform: translate(-100%, -100%);
  }

  &--text-on-checkpoint &__step-text--under:first-of-type {
    transform: translate(0, 100%);
  }

  &--text-on-checkpoint &__step-text--under:last-of-type {
    transform: translate(-100%, 100%);
  }

  &__options {
    padding-top: 50px;
    display: grid;
    gap: 10px;
  }

  &__option-label {
    user-select: none;
  }

  &__option-label,
  &__option-checkbox {
    cursor: pointer;
  }

  &__option-label:hover &__option-input {
    opacity: 0.7;
  }

  &__option-input {
    max-width: 40px;
  }
}
</style>