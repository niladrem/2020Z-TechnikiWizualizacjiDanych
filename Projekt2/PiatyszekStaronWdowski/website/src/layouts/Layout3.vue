<template>
  <div class="layout">
    <SlotContainer v-for="n in plots" :class="'mainplot' + n" :slotv="slots[n-1]" :pinned="!!pinned[n-1]" :key="n"
    :largeWindow="n===1" isPlaceholder pinable isMovable
    @pin="$emit('pin', n-1)" @swap="$emit('swap', $event)" @addSlot="$emit('addSlot', $event)"
    @merged="$emit('merged', Object.assign({ n: n-1 }, $event))" @openPage="$emit('openPage', $event)" />
  </div>
</template>

<script>
import SlotContainer from '@/components/SlotContainer.vue'

export default {
  name: 'Layout3',
  props: {
    slots: Array,
    pinned: Object,
    layoutPlots: Number
  },
  model: {
    prop: 'layoutPlots',
    event: 'update'
  },
  computed: {
    plots () { return Math.min(3, this.slots.length) }
  },
  watch: {
    plots: {
      handler () { this.$emit('update', this.plots) },
      immediate: true
    }
  },
  components: {
    SlotContainer
  }
}
</script>

<style scoped>
.layout {
  grid-template-columns: 3fr 1fr;
  grid-template-rows: 50% 50%;
  display: grid;
  grid-template-areas:
    "p1 p2"
    "p1 p3"
}
.mainplot1 {
  grid-area: p1;
}
.mainplot2 {
  grid-area: p2;
}
.mainplot3 {
  grid-area: p3;
}
</style>
