<template>
  <div id="app" class="app">
    <app-timeline class="app__timeline"
                  :counts="9"
                  :process-count="3"
                  :is-text-under="false"
                  :is-text-center="true"
    />
    <app-timeline class="app__timeline"
                  :counts="4"
                  :process-count="3"
                  :is-text-under="false"
    />
    <app-timeline class="app__timeline"
                  :counts="2"
                  :process-count="1"
                  :is-text-under="false"
    />
  </div>
</template>

<script>
import AppTimeline from "./components/timeline";

export default {
  name: 'App',
  components: {
    AppTimeline,
  },
}
</script>

<style lang="scss" scoped>
.app {
  display: flex;
  flex-direction: column;
  padding: 40px;

  &__timeline:not(:last-child) {
    margin-bottom: 60px;
  }
}
</style>
