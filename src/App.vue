<template>
  <div id="app" class="app">
    <app-timeline class="app__timeline"
                  :counts="4"
                  :active-checkpoint="3"
    />
    <app-timeline class="app__timeline"
                  :counts="4"
                  :active-checkpoint="3"
                  :is-text-center-progress="true"
    />
    <app-timeline class="app__timeline"
                  :counts="4"
                  :active-checkpoint="3"
                  :is-text-under="true"
    />
    <app-timeline class="app__timeline"
                  :counts="4"
                  :active-checkpoint="3"
                  :is-text-under="true"
                  :is-text-center-progress="true"
    />
  </div>
</template>

<script>
import AppTimeline from './components/timeline';

export default {
  name: 'App',
  components: {
    AppTimeline,
  },
}
</script>

<style lang="scss" scoped>
.app {
  display: grid;
  padding: 40px;
  gap: 180px;

  &__timeline {
    grid-column: 1;
  }
}
</style>
