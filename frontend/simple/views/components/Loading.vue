<template>
  <div class="c-loading has-text-centered"
    :class="{ 'c-fullView': theme === 'fullView' }">
    <h1 class="title is-3 is-marginless">{{Ltitle}}></h1>
    <p>{{Ltext}}</p>
  </div>
</template>
<style lang="scss" scoped>
@import "../../assets/sass/theme/index";

.c-loading {
  opacity: 0;
  padding: $gi-spacer-lg 0;
  animation: showLoading 200ms 500ms forwards;

  &.c-fullView {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
}

@keyframes showLoading {
  from { opacity: 0; }
  to { opacity: 1; }
}
</style>
<script>
export default {
  name: 'Loading',
  props: {
    title: {
      default: 'Hang in there',
      type: String
    },
    text: {
      default: 'Magic is happening...',
      type: [String, Number]
    },
    theme: {
      validator (value) {
        return ['fullView', ''].indexOf(value) !== -1
      }
    }
  },
  computed: {
    Ltitle () {
      return this.L(this.title)
    },
    Ltext () {
      return this.L(this.text)
    }
  }
}
</script>
